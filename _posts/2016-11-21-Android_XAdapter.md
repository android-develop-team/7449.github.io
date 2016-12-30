---
layout:     post
title:      "Android_XAdapter"
subtitle:   "BaseRVAdapter,支持下拉刷新加载和添加多个header和footer的RecyclerViewAdapter"
date:       2016-11-12
author:     "y"
header-mask: 0.3
header-img: "img/android.jpg"
catalog: true
tags:
    - android
    - RecyclerView
    - Adapter
---



## Screenshots

![](https://github.com/7449/XAdapter/blob/master/xadapter.gif)

[https://github.com/7449/XAdapter/blob/master/xadapter.gif](https://github.com/7449/XAdapter/blob/master/xadapter.gif)

## gradle

>compile 'com.xadapter:xadapter:0.0.6'


## 完整示例


        recyclerView.setAdapter(
                xRecyclerViewAdapter
                        .initXData(mainBeen)
                        .addRecyclerView(recyclerView)
                        .setLayoutId(R.layout.item)
                        .setRefreshProgressStyle(ProgressStyle.BallSpinFadeLoader)
                        .setLoadingMoreProgressStyle(ProgressStyle.BallRotate)
                        .setImageView(R.drawable.iconfont_downgrey)
                        .setHeaderBackgroundColor(R.color.colorBlack)
                        .setFooterBackgroundColor(R.color.colorBlack)
                        .setHeaderTextColor(R.color.textColor)
                        .setFooterTextColor(R.color.textColor)
                        .setPullRefreshEnabled(true)
                        .setLoadingMoreEnabled(true)
                        .addHeaderView(LayoutInflater.from(this).inflate(R.layout.item_header_1, (ViewGroup) findViewById(android.R.id.content), false))
                        .addHeaderView(LayoutInflater.from(this).inflate(R.layout.item_header_2, (ViewGroup) findViewById(android.R.id.content), false))
                        .addHeaderView(LayoutInflater.from(this).inflate(R.layout.item_header_3, (ViewGroup) findViewById(android.R.id.content), false))
                        .addFooterView(LayoutInflater.from(this).inflate(R.layout.item_footer_1, (ViewGroup) findViewById(android.R.id.content), false))
                        .addFooterView(LayoutInflater.from(this).inflate(R.layout.item_footer_2, (ViewGroup) findViewById(android.R.id.content), false))
                        .addFooterView(LayoutInflater.from(this).inflate(R.layout.item_footer_3, (ViewGroup) findViewById(android.R.id.content), false))
                        .onXBind(this)
                        .setOnLongClickListener(this)
                        .setOnItemClickListener(this)
                        .setLoadingListener(this)
                        .setFooterListener(this)
                        .setRefreshing(true)
        );

onXBind  
这里进行数据的展示

    @Override
    public void onXBind(XViewHolder holder, int position, MainBean mainBean) {
        holder.setTextView(R.id.tv_name, mainBean.getName());
        holder.setTextView(R.id.tv_age, mainBean.getAge() + "");
    }

## emptyView

>setEmptyView一定要在addRecyclerView之前调用，否则无效，具体的内容可以看simple以及源码
>是否显示由用户自己手动决定，在网络异常或者数据为空的时候调用xRecyclerViewAdapter.isShowEmptyView();具体情况simple有例子

	 recyclerView.setAdapter(
	                xRecyclerViewAdapter
	                        .initXData(mainBean)
	                        .setEmptyView(findViewById(R.id.emptyView))
	                        .addRecyclerView(recyclerView)
	                        .setLayoutId(R.layout.item)
	                        .setOnXEmptyViewListener(new XBaseAdapter.OnXEmptyViewListener() {
	                            @Override
	                            public void onXEmptyViewClick(View view) {
	                                Toast.makeText(EmptyViewActivity.this, "emptyView", Toast.LENGTH_SHORT).show();
	                            }
	                        })
	        );

点击事件可以自己用emptyView实现，但是建议使用XRecyclerViewAdapter 的 onXEmptyViewListener来实现emptyView的点击事件

## 下拉刷新和上拉加载

默认不打开，如果有必要，请手动打开，并调用addRecyclerView

                xRecyclerViewAdapter
					.initXData(mainBean)
	                .setLayoutId(R.layout.item)
	                .addRecyclerView(recyclerView)
	                .setPullRefreshEnabled(true)
	                .setPullRefreshEnabled(true)
	                .setLoadingListener(new XBaseAdapter.LoadingListener() {
	                    @Override
	                    public void onRefresh() {
	                        
	                    }
	
	                    @Override
	                    public void onLoadMore() {
	
	                    }
	                })

下拉刷新完成之后

这取决于用户选择刷新是否失败或成功

>xRecyclerViewAdapter.refreshComplete(BaseRefreshHeader.STATE_DONE);


上拉加载完成之后

这取决于用户选择加载是否失败或成功

>xRecyclerViewAdapter.loadMoreComplete(XFooterLayout.STATE_NOMORE);


## 添加header和footer

		xRecyclerViewAdapter
		 .addHeaderView(LayoutInflater.from(this).inflate(R.layout.item_header_1, (ViewGroup) findViewById(android.R.id.content), false))
		 .addFooterView(LayoutInflater.from(this).inflate(R.layout.item_footer_1, (ViewGroup) findViewById(android.R.id.content), false))

## 加载动画

XAdapter 的刷新头部以及底部都是来自 [XRecyclerView](https://github.com/jianghejie/XRecyclerView), 所以 [XRecyclerView](https://github.com/jianghejie/XRecyclerView) 支持的动画XAdapter 都支持,并且对Layout进行扩展，可以设置背景色和字体色，字体大小

              	 xRecyclerViewAdapter
                        .initXData(mainBeen)
                        .addRecyclerView(recyclerView)
                        .setLayoutId(R.layout.item)
                        .setRefreshProgressStyle(ProgressStyle.BallSpinFadeLoader)
                        .setLoadingMoreProgressStyle(ProgressStyle.BallRotate)
                        .setImageView(R.drawable.iconfont_downgrey)
                        .setHeaderBackgroundColor(R.color.colorBlack)
                        .setFooterBackgroundColor(R.color.colorBlack)
                        .setHeaderTextColor(R.color.textColor)
                        .setFooterTextColor(R.color.textColor)


## 感谢

[https://github.com/jianghejie/XRecyclerView](https://github.com/jianghejie/XRecyclerView)