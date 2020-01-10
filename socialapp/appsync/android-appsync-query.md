---
title: "List up posts "
chapter: false
date: 2018-08-07T08:30:11-07:00
weight: 20
---

Now we will list up posts posted by the user on the main screen.

In the onCreate function of MainActivity.java, create an AWSAppSyncClient using the ClientFactory.

Copy ClientFactory.appSyncInit (...) into onCreate () function as below.

```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        checkPermission();
        //appsync
        ClientFactory.appSyncInit(getApplicationContext());
        ...
  }
```



Add code to onResume () to call queryList () as shown below.

```java
    protected void onResume() {
        super.onResume();
        //appsync
        queryList();
    }
```

Add a queryList () function and a callback function to get the required query result as shown below.

```java
    private PostAdapter mAdapter;

public void queryList() {
        ClientFactory.getAppSyncClient().query(ListPostsQuery.builder()
                .id("DEV-DAY")
                .sortDirection(ModelSortDirection.DESC)
                .build())
                .responseFetcher(AppSyncResponseFetchers.CACHE_AND_NETWORK)
                .enqueue(queryCallback);
    }
    private ArrayList<ListPostsQuery.Item> mItems;

    private GraphQLCall.Callback<ListPostsQuery.Data> queryCallback = new GraphQLCall.Callback<ListPostsQuery.Data>() {

        @Override
        public void onResponse(@Nonnull Response<ListPostsQuery.Data> response) {
            Log.e(TAG, response.data().listPosts().items().toString());
            mItems = new ArrayList<>(response.data().listPosts().items());

            /* Log.i(TAG, "Retrieved list items: " + mItems.toString()); */

            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    mAdapter.setItems(mItems);
                    mAdapter.notifyDataSetChanged();
                }
            });
        }

        @Override
        public void onFailure(@Nonnull ApolloException e) {
						e.printStackTrace();
        }
    };
```

Import the required class.

```java
import com.amazonaws.amplify.generated.graphql.ListPostsQuery;
import com.amazonaws.mobileconnectors.appsync.fetcher.AppSyncResponseFetchers;
import com.apollographql.apollo.GraphQLCall;
import com.apollographql.apollo.api.Response;
import com.apollographql.apollo.exception.ApolloException;
import java.util.ArrayList;
import javax.annotation.Nonnull;
import type.ModelSortDirection;

```
Note : In Android Studio, syntax that requires import displays an error in red. Move the cursor to the location, press the [MAC] option key and the Enter key ([Window] Alt key and Enter key) at the same time and select the import class. The required class is automatically imported.

<img src="/images/optionenter.png" width="80%" hight="80%">
<img src="/images/importclass.png" width="80%" hight="80%">


Posts are listup via RecyclerView. Create a PostAdapter class for use with the RecyclerView.

PostAdapter.java 

```java
package com.example.socialandroidapp;


import android.content.Context;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.Button;
import android.widget.ImageView;
import android.widget.TextView;

import com.amazonaws.ClientConfiguration;
import com.amazonaws.Protocol;
import com.amazonaws.amplify.generated.graphql.ListPostsQuery;
import com.amazonaws.services.s3.AmazonS3Client;
import com.squareup.picasso.Picasso;

import java.net.URL;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;


public class PostAdapter extends RecyclerView.Adapter<PostAdapter.Holder> {

    private static String TAG = "dev-day-item";
    private LayoutInflater mInflater;
    private List<ListPostsQuery.Item> mData = new ArrayList<>();
    private Context ctx;

    PostAdapter(Context context) {
        ctx = context;
        this.mInflater = LayoutInflater.from(context);
    }

    @NonNull
    @Override
    public PostAdapter.Holder onCreateViewHolder(@NonNull ViewGroup viewGroup, int i) {
        View view = mInflater.inflate(R.layout.post_item, viewGroup, false);
        Holder holder = new Holder(view);
        return holder;
    }


    @Override
    public void onBindViewHolder(@NonNull final PostAdapter.Holder holder, int i) {
        holder.bindData(mData.get(i));

        ListPostsQuery.Photo so = mData.get(i).photo();

        String bucketName = so.fragments().s3Object().bucket();
        String pName = so.fragments().s3Object().key();

        ClientConfiguration clientConfig = new ClientConfiguration();
        clientConfig.setProtocol(Protocol.HTTP);

        AmazonS3Client s3 = new AmazonS3Client(ClientFactory.getAWSCredentials(), clientConfig);

        long d = System.currentTimeMillis() + (2 * 24 * 60 * 60 * 1000);

        URL url = s3.generatePresignedUrl(bucketName, pName, new Date(d));

        final String tmpStr = url.toString();
        Log.e(TAG, "URL = " + tmpStr);

        //new DownloadImageFromInternet(ctx, holder.iv).execute(tmpStr);
        Picasso.get().load(tmpStr).into(holder.iv);
    }

    @Override
    public int getItemCount() {
        return mData.size();
    }

    public void setItems(List<ListPostsQuery.Item> items) {
        mData = items;
    }



    public class Holder extends RecyclerView.ViewHolder {
        private TextView writerTxt, contentsTxt, titleTxt;

        private ImageView iv;
        private Button translateBtn;

        public Holder(View view) {
            super(view);

            iv = view.findViewById(R.id.contentImg);
            writerTxt = view.findViewById(R.id.writer);
            contentsTxt = view.findViewById(R.id.contents);
            titleTxt = view.findViewById(R.id.title);
            translateBtn = view.findViewById(R.id.translateBtn);
        }

        void bindData(final ListPostsQuery.Item item) {

            writerTxt.setText(item.author());
            contentsTxt.setText(item.content());
            titleTxt.setText(item.title());


        }
    }
}

```

Create and link PostAdapter for RecyclerView.

Write the following at the bottom of onCreate () function of MainActivity.java

```java
import androidx.recyclerview.widget.LinearLayoutManager;

protected void onCreate(Bundle savedInstanceState) {
  ...
    mAdapter = new PostAdapter(getApplicationContext());

    recyclerView = findViewById(R.id.itemlist);
    recyclerView.setLayoutManager(new LinearLayoutManager(this));
    recyclerView.setHasFixedSize(true);
    recyclerView.setAdapter(mAdapter);
}
```

Now when you run the app, you'll see the first post you made in Posting.

<img src="/images/main-list.png" width="30%" hight="30%">
