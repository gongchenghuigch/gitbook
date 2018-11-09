# FlatList使用详解及源码解析

## 前言

长列表或者无限下拉列表是最常见的应用场景之一。RN 提供的 ListView 组件，在长列表这种数据量大的场景下，性能堪忧。而在最新的 0.43 版本中，提供了 FlatList 组件，或许就是你需要的高性能长列表解决方案。它足以应对大多数的长列表场景。



## 一、功能简介

**FlatList**高性能的简单列表组件，支持下面这些常用的功能：

- 完全跨平台。
- 支持水平布局模式。
- 行组件显示或隐藏时可配置回调事件。
- 支持单独的头部组件。
- 支持单独的尾部组件。
- 支持自定义行间分隔线。
- 支持下拉刷新。
- 支持上拉加载。
- 支持跳转到指定行（ScrollToIndex）。



## 二、属性说明

FlatList 有三个核心属性 `data` `renderItem` `getItemLayout`。它继承自 ScrollView 组件，所以拥有 ScrollView 的属性和方法。



#### data

和 ListView 不同，它没有特殊的 `DataSource` 数据类型作为传入参数。它接收的仅仅只是一个 `Array<object>` 作为参数。
参数数组中的每一项，需要包含 `key` 值作为唯一标示。数据结构如下：

```
[
    {
        infoId: "1030296374794543105",
        price: "31万",
        warnType: 0,
        spdz: "",
        postDate: "08-17",
        state: 2,
        syncResult: "已同步2 失败0 同步中0",
        pic: "/p1/tiny/n_v2862e29d1f2854bbebf208d0a520591a9.jpg",
        title: "丰田 普拉多 2010款 4.0L 自动TX",
        age: "库龄35天",
        intention: 0
    }
]
```



#### renderItem

它接收一个函数作为参数，该函数返回一个 ReactElement。函数的第一个参数的 `item` 是 `data`属性中的每个列表的数据（ `Array<object>` 中的 `object`) 。这样就将列表元素和数据结合在一起，生成了列表，示例：

```
_renderItem({ item, index }) {
    let { infoId, title, pic, price, syncResult, postDate, age, state } = item;
    let url = "https://pic2.58cdn.com.cn" + pic;
    let icon = {
        2: require("./../img/yishou.png"),
        0: require("./../img/tuiku.png")
    };
    return (
        <TouchableHighlight style={styles.carList}>
            <View style={styles.carView}>
                {state == 1 ? null : 
                <Image style={styles.carIcon} source={icon[state]} />}
                <Image style={styles.carImg} source={{ uri: url }} />
                <View style={styles.carInfo}>
                    <Text numberOfLines={1} style={styles.carTitle}>
                        {title}
                    </Text>
                    <View style={styles.carTime}>
                        <Text style={styles.carDate}>{postDate}</Text>
                        <Text style={styles.carAge}>{age}</Text>
                    </View>
                    <View style={styles.carState}>
                        <Text style={styles.carPrice}>{price}</Text>
                        <Text style={styles.carStatus}>{syncResult}</Text>
                    </View>
                </View>
            </View>
        </TouchableHighlight>
    );
}
```



#### getItemLayout

可选优化项。但是实际测试中，如果不做该项优化，性能会差很多。所以强烈建议做此项优化！
如果不做该项优化，每个列表都需要事先渲染一次，动态地取得其渲染尺寸，然后再真正地渲染到页面中。

如果预先知道列表中的每一项的高度(ITEM_HEIGHT)和其在父组件中的偏移量(offset)和位置(index)，就能减少一次渲染。这是很关键的性能优化点

```
getItemLayout={(data, index) =>
    // 90 是被渲染 item 的高度 ITEM_HEIGHT。
    ({ length: 90, offset: 90 * index, index })
}
```



其他属性介绍：

#### extraData

如果有除data以外的数据用在列表中（不论是用在renderItem
还是Header或者Footer中），请在此属性中指定。同时此数据在修改时也需要先修改其引用地址（比如先复制到一个新的Object或者数组中），然后再修改其值，否则界面很可能不会刷新。（非常重要）

```
//确保state更新时列表及时更新
extraData={this.state}
```



#### ListEmptyComponent

列表为空时渲染该组件。可以是React Component, 也可以是一个render函数， 或者渲染好的element。

```
_renderEmptyView = () => {
    return (
        <View style={styles.listEmpty}>
            <Image style={styles.listEmptyImg} 
            source={require("./../img/norecord.png")} />
            <Text style={styles.listEmptyText}>您暂无符合此筛选条件的车源</Text>
        </View>
    );
};
```



#### keyExtractor

此函数用于为给定的item生成一个不重复的key。Key的作用是使React能够区分同类元素的不同个体，以便在刷新时能够确定其变化的位置，减少重新渲染的开销。若不指定此函数，则默认抽取`item.key`作为key值。若`item.key`也不存在，则使用数组下标。

```
keyExtractor={(item, index) => item.infoId}
```



示例：



<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/flatlist.png" width="375"/>

#### 下拉刷新、上拉加载

> onRefresh={() => this._onRefresh()}
>
>  refreshing={this.state.isRefresh}
>
> //加载更多
>
> onEndReached={() => this._onLoadMore()}
>
> //当列表被滚动到距离内容最底部不足onEndReachedThreshold的距离时调用
>
> onEndReachedThreshold={0.1}

refreshing

在等待加载新数据时将此属性设为true，列表就会显示出一个正在加载的符号。

onRefresh

如果设置了此选项，则会在列表头部添加一个标准的RefreshControl
控件，以便实现“下拉刷新”的功能。同时你需要正确设置refreshing
属性。

onEndReachedThreshold

决定当距离内容最底部还有多远时触发onEndReached
回调。注意此参数是一个比值而非像素单位。比如，0.5表示距离内容最底部的距离为当前列表可见长度的一半时触发。

onEndReached

当列表被滚动到距离内容最底部不足onEndReachedThreshold
的距离时调用。

```
 constructor(props) {
        super(props);
        this.page = 1;
        this.state = {
            // 下拉刷新
            isRefresh: false,
            // 加载更多
            isLoadMore: false
        };
    }
 _onRefresh() {
    // 不处于 下拉刷新
    if (!this.state.isRefresh) {
        this.page = 1;
        let request = {
            cateId: 29,
            pageNum: this.page,
            pageSize: 20
        };
        request = this.mergeJson(request, this.state.filterVaule);
        this.getSourceList(request);
    }
}
getSourceList(request) {
    WBCST.getFetch("https://***.com/ershouche/stock/getList", request).then(
        (response) => {
            if (response && response.respCode == 0 && response.respData) {
                if (this.page == 1) {
                    //重新渲染
                    this.setState({
                        data: response.respData
                    });
                } else {
                    let loadMore = false;
                    if (response.respData.length < request.pageSize) {
                        //显示到底部了 没有更多数据了
                        loadMore = true;
                    }
                    this.setState({
                        //加载更多 这个变量不刷新
                        isLoadMore: loadMore,
                        //添加新数据源
                        data: this.state.data.concat(response.respData)
                    });
                }
            } else {
                // WBCST.loading({ state: "dismiss" });
                this.setState({
                    data: stockList.respData
                    // data: []
                });
                WBCST.toast({ text: "当前网络不可用，请检查网络设置！", state: "error" });
            }
        }
    );
}
```

示例：

下拉刷新

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/flatlist1.png" width="375"/>

上拉加载

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/flatlist3.png" width="375" />



## 三、数据对比

性能瓶颈主要体现在 Android 这边，测试无限下拉列表，列表为常见的左文右图的形式。

测试数据如下：

| 对比         | ListView | FlatList |
| ------------ | -------- | -------- |
| 1000条时内存 | 350M     | 180M     |
| 2000条时内存 | /        | 230M     |
| js-fps       | 4~6 fps  | 8~20 fps |

`js-pfs` 类似于游戏的画面渲染的帧率，60 为最高。它用于判断 js 线程的繁忙程度，数值越大说明 js 线程运行状态越好，数值越小说明 js 线程运行状态越差。在快速滑动测试 ListView 的时候， `js-pfs` 的值一直在 4~6 范围波动，即使停止滑动，`js-pfs` 的值也不能很快恢复正常。而 FlatList 在快速滚动后停止，`js-pfs` 能够很快的恢复到正常。

内存方面，ListView 滑动到 1000 条时，已经涨到 350M。这时机器已经卡的不行了，所以没法滑到 2000 条并给出相关数据。而 FlatList 滑到 2000 条时的内存，也比 ListView 1000 条时的内存少不少。说明，FlatList 对内存的控制是很优秀的。

主观体验方面：FlatList 快速滑动至 2000 条的过程中全程体验流畅，没有出现卡顿或肉眼可见的掉帧现象。而ListView 滑动到 200 条开始卡顿，页面滑动变得不顺畅，到 500 条渲染极其缓慢，到 1000 条时已经滑不动了。

通过以上的简单的测试，可以看出，FlatList 已经能够应对简单的无限列表的场景。



## 四、源码分析

ReactNative列表是基于ScrollView的，并没有直接使用IOS或Android的原生列表组件。因为RN真正调用native代码是异步的，并不能保证同步，而在native环境中，所有即将在视窗中呈现的元素都必须同步渲染，超过一定的时间（ios为16ms）就会出现掉帧，所以RN采用ScrollView作为列表组件的基础。 

ReactNative刚开始提供的是ListView组件，在数据量大的情况下，性能特别差。目前提供的列表组件是FlatList和SectionList，性能问题得到了很好的解决，它们都是基于VirtualizedList。

源码：node_modules/react-native/Libraries/Lists/

#### Flatlist底层源码

```
render() {
    if (this.props.legacyImplementation) {
      return (
        <MetroListView
          {...this.props}
          items={this.props.data}
          ref={this._captureRef}
        />
      );
    } else {
      return (
        <VirtualizedList
          {...this.props}
          renderItem={this._renderItem}
          getItem={this._getItem}
          getItemCount={this._getItemCount}
          keyExtractor={this._keyExtractor}
          ref={this._captureRef}
          viewabilityConfigCallbackPairs={this._virtualizedListPairs}
        />
      );
    }
}
```

legacyImplementation设置为true时使用旧版listView 默认是false

#### VirtualizedList原理

- 每次新增绘制item的最大数量为10，循环绘制（以10为单位累加绘制）；
- 首先绘制显示在屏幕中的items，再根据优先级循环绘制屏幕上显示items相近的数据，直至绘制完成；
- 每次绘制过程中，所有不需要绘制的元素用空View代替

```
const {
    ListEmptyComponent,//数据为空显示样式
    ListFooterComponent,//footer
    ListHeaderComponent,//header
} = this.props;
const {data, horizontal} = this.props;
const isVirtualizationDisabled = this._isVirtualizationDisabled();
//确定反转样式
const inversionStyle = this.props.inverted
    ? this.props.horizontal ? styles.horizontallyInverted : styles.verticallyInverted
    : null;
const cells = [];//list显示view集合
//section集合（即SectionList中的分组）
const stickyIndicesFromProps = new Set(this.props.stickyHeaderIndices);
const stickyHeaderIndices = [];
//处理header
if (ListHeaderComponent) {
    if (stickyIndicesFromProps.has(0)) {
        stickyHeaderIndices.push(0);
    }
    const element = React.isValidElement(ListHeaderComponent)
        ? (ListHeaderComponent)
        : (<ListHeaderComponent/>);
    cells.push(
        <View
            key="$header"
            onLayout={this._onLayoutHeader}
            style={inversionStyle}>
            {element}
        </View>,
    );
}
//获取数据数量
const itemCount = this.props.getItemCount(data);
if (itemCount > 0) {
    _usedIndexForKey = false;
    const spacerKey = !horizontal ? 'height' : 'width';
    //计算初始化末尾值，有初始化值为-1，无初始化值为默认9
    const lastInitialIndex = this.props.initialScrollIndex
        ? -1
        : this.props.initialNumToRender - 1;
    const {first, last} = this.state;
    //绘制初始化items
    this._pushCells(
        cells,
        stickyHeaderIndices,
        stickyIndicesFromProps,
        0,
        lastInitialIndex,
        inversionStyle,
    );
    const firstAfterInitial = Math.max(lastInitialIndex + 1, first);
    //非0初始化（即：initialScrollIndex值不为0）
    if (!isVirtualizationDisabled && first > lastInitialIndex + 1) {
        let insertedStickySpacer = false;
        if (stickyIndicesFromProps.size > 0) {
            const stickyOffset = ListHeaderComponent ? 1 : 0;
            //如果存在section，绘制出section和section中的内容，用多个空view代替
            // See if there are any sticky headers in the virtualized space that we need to render.
            for (let ii = firstAfterInitial - 1; ii > lastInitialIndex; ii--) {
               //...代码省略...
            }
        }
        //不存在section，直接绘制一个空view
        if (!insertedStickySpacer) {
            const initBlock = this._getFrameMetricsApprox(lastInitialIndex);
            const firstSpace = this._getFrameMetricsApprox(first).offset
                - (initBlock.offset + initBlock.length);
            //从第 11 个 items (除去初始化的 10个 items) 到 first 渲染空白元素    
            cells.push(
                <View key="$lead_spacer" style={{[spacerKey]: firstSpace}}/>,
            );
        }
    }
    // last 是最后一个在视图(包括要即将在视图)中的元素。
	// 从 firstAfterInitial 到 last ，即用户看到的界面渲染真正的 item
    this._pushCells(
        cells,
        stickyHeaderIndices,
        stickyIndicesFromProps,
        firstAfterInitial,
        last,
        inversionStyle,
    );
    if (!this._hasWarned.keys && _usedIndexForKey) {
        this._hasWarned.keys = true;
    }
    //last小于itemCount（本次刷新过程中不用显示的元素用一个空View代替）
    if (!isVirtualizationDisabled && last < itemCount - 1) {
        const lastFrame = this._getFrameMetricsApprox(last);
        const end = this.props.getItemLayout
            ? itemCount - 1
            : Math.min(itemCount - 1, this._highestMeasuredFrameIndex);
        const endFrame = this._getFrameMetricsApprox(end);
        const tailSpacerLength =
            endFrame.offset +
            endFrame.length -
            (lastFrame.offset + lastFrame.length);
            // last 之后的元素，渲染空白
        cells.push(
            <View key="$tail_spacer" style={{[spacerKey]: tailSpacerLength}}/>,
        );
    }
} else if (ListEmptyComponent) {//没有数据的话显示 空样式
    const element = React.isValidElement(ListEmptyComponent) ? (
        ListEmptyComponent
    ) : (
        // $FlowFixMe
        <ListEmptyComponent/>
    );
    cells.push(
        <View
            key="$empty"
            onLayout={this._onLayoutEmpty}
            style={inversionStyle}>
            {element}
        </View>,
    );
}
//footer
if (ListFooterComponent) {
    const element = React.isValidElement(ListFooterComponent)
        ? (ListFooterComponent)
        : (<ListFooterCo mponent/>);
    cells.push(
        <View
            key="$footer"
            onLayout={this._onLayoutFooter}
            style={inversionStyle}>
            {element}
        </View>,
    );
}
//事件...
const scrollProps = {
    ...this.props,
    onContentSizeChange: this._onContentSizeChange,
    onLayout: this._onLayout,
    onScroll: this._onScroll,
    onScrollBeginDrag: this._onScrollBeginDrag,
    onScrollEndDrag: this._onScrollEndDrag,
    onMomentumScrollEnd: this._onMomentumScrollEnd,
    scrollEventThrottle: this.props.scrollEventThrottle, // TODO: Android support
    stickyHeaderIndices,
};
if (inversionStyle) {
    scrollProps.style = [inversionStyle, this.props.style];
}
//将列表元素添加到ScrollView中
const ret = React.cloneElement(
    (this.props.renderScrollComponent || this._defaultRenderScrollComponent)(
        scrollProps,
    ),
    {
        ref: this._captureScrollRef,
    },
    cells,
);
if (this.props.debug) {
    return (
        <View style={{flex: 1}}>
            {ret}
            {this._renderDebugOverlay()}
        </View>
    );
} else {
    return ret;
}
```

父组件传入属性：

#### getItemLayout

用于避免动态测量内容尺寸的开销，如 getItemLayout={(data, index) => ( {length: 行高, offset: 行高 * index, index} )}

#### _onCellLayout

 就是用于动态计算元素高度的方法，如果事先知道元素的高度和位置，就可以使用上面提到的 `getItemLayout` 方法，就能跳过 `_onCellLayout` 这一步，获得更好的性能。

```
_onCellLayout(e, cellKey, index) {
    const layout = e.nativeEvent.layout;
    const next = {
      offset: this._selectOffset(layout),
      length: this._selectLength(layout),
      index,
      inLayout: true,
    };
    const curr = this._frames[cellKey];
    if (
      !curr ||
      next.offset !== curr.offset ||
      next.length !== curr.length ||
      index !== curr.index
    ) {
      this._totalCellLength += next.length - (curr ? curr.length : 0);
      this._totalCellsMeasured += curr ? 0 : 1;
      this._averageCellLength =
        this._totalCellLength / this._totalCellsMeasured;
      this._frames[cellKey] = next;
      this._highestMeasuredFrameIndex = Math.max(
        this._highestMeasuredFrameIndex,
        index,
      );
      this._scheduleCellsToRenderUpdate();
    } else {
      this._frames[cellKey].inLayout = true;
    }
    this._computeBlankness();
  }
```

#### _pushCells

将数据源中的first—last之间的数据绑定到view上，添加到列表数据中

#### initialScrollIndex

开始时屏幕顶端的元素是列表中的第 initialScrollIndex 个元素, 而不是第一个元素



#### 绘制过程

1. 从state中获取{first,last}。每次绘制就是绘制在数据源data中下标为first—last这部分数据。

   ```
   type State = {first: number, last: number};
   let initialState = {
     first: this.props.initialScrollIndex || 0,
     last:
       Math.min(
         this.props.getItemCount(this.props.data),
         (this.props.initialScrollIndex || 0) + this.props.initialNumToRender,
       ) - 1,
   };
   ```

2. 添加0—lastInitialIndex元素。当initialScrollIndex为正整数，该部分不会添加任何元素；若未定义initialScrollIndex或为0的时候，默认添加0—9数据。

3. 添加可见区域上方空View。当initialScrollIndex为正整数，将添加空白View代替可见区域上方的内容。

4. 添加firstAfterInitial—last元素；当initialScrollIndex为正整数，该部分添加的为first—last数据；若未定义initialScrollIndex或为0的时候，添加的为10-last数据。

5. 添加可见区域下方空View。若本次没有将之后的数据绘制完成，将添加空白View进行代替。

#### 如何进行下一次绘制？

循环绘制

在每次刷新完成后会调用_scheduleCellsToRenderUpdate方法，该方法最终会调用updateCellsToRender方法。

```
componentDidUpdate() {
	this._scheduleCellsToRenderUpdate();
}
```

在_updateCellsToRender中会调用setState方法更新状态。所以在每次绘制完成（状态更新完成）后，都会接着调用更新方法，所以形成了循环绘制的效果。理论上这种结构会造成无限循环，但是VirtualizedList是继承自PureComponent，所以当检测到状态未改变的时候就会终止更新。

```
 _updateCellsToRender = () => {
    const {data, getItemCount, onEndReachedThreshold} = this.props;
    const isVirtualizationDisabled = this._isVirtualizationDisabled();
    this._updateViewableItems(data);
    if (!data) {
      return;
    }
    this.setState(state => {
      let newState;
      if (!isVirtualizationDisabled) {
         //如果我们用虚假数据运行它，我们将强制渲染窗口{first：0，last：0}，
         //并清除initialNumToRender渲染元素。所以让我们等到滚动视图指标设置完毕。 
        if (this._scrollMetrics.visibleLength) {
          //如果我们有一个非零的initialScrollIndex并在我们滚动之前运行它，
           //我们将从initialScrollIndex开始清除initialNumToRender渲染元素。
           //所以让我们等到我们将视图滚动到正确的位置。
          if (!this.props.initialScrollIndex || this._scrollMetrics.offset) {
            newState = computeWindowedRenderLimits(
              this.props,
              state,
              this._getFrameMetricsApprox,
              this._scrollMetrics,
            );
          }
        }
      } else {
        const {contentLength, offset, visibleLength} = this._scrollMetrics;
        const distanceFromEnd = contentLength - visibleLength - offset;
        const renderAhead =
          /* $FlowFixMe(>=0.63.0 site=react_native_fb) This comment suppresses
           * an error found when Flow v0.63 was deployed. To see the error
           * delete this comment and run Flow. */
          distanceFromEnd < onEndReachedThreshold * visibleLength
            ? this.props.maxToRenderPerBatch
            : 0;
        newState = {
          first: 0,
          last: Math.min(state.last + renderAhead, getItemCount(data) - 1),
        };
      }
      if (newState && this._nestedChildLists.size > 0) {
        const newFirst = newState.first;
        const newLast = newState.last;
        // If some cell in the new state has a child list in it, we should only render
        // up through that item, so that we give that list a chance to render.
        // Otherwise there's churn from multiple child lists mounting and un-mounting
        // their items.
        for (let ii = newFirst; ii <= newLast; ii++) {
          const cellKeyForIndex = this._indicesToKeys.get(ii);
          const childListKeys =
            cellKeyForIndex &&
            this._cellKeysToChildListKeys.get(cellKeyForIndex);
          if (!childListKeys) {
            continue;
          }
          let someChildHasMore = false;
          // For each cell, need to check whether any child list in it has more elements to render
          for (let childKey of childListKeys) {
            const childList = this._nestedChildLists.get(childKey);
            if (childList && childList.ref && childList.ref.hasMore()) {
              someChildHasMore = true;
              break;
            }
          }
          if (someChildHasMore) {
            newState.last = ii;
            break;
          }
        }
      }
      return newState;
    });
  };
```





简单分析源码可总结Flatlist一些性能：

> FlatList 之所以节约内存、渲染快，是因为它只将用户看到的(和即将看到的)部分真正渲染出来了。而用户看不到的地方，渲染的只是空白元素。渲染空白元素相比渲染真正的列表元素需要内存和计算量会大大减少，这就是性能好的原因。

缺点就是，用户可能滚动太快，空白还没被渲染成真的 items，就被看见了。

FlatList 将页面分为 4 部分。

初始化部分：在每次都会渲染

上方空白部分

展现部分

下方空白部分

当用户滚动时，根据需求动态的调整(上下)空白部分的高度，并将视窗中的列表元素正确渲染出来。

由RN的源码可看出它并没有和 native 端复用逻辑。而且如果有些机器性能极差，渲染过慢，那些假的列表——空白元素就会被用户看到！

是基于ScrollView 组件进行性能优化

```
return (
    <ScrollView
      {...props}
      refreshControl={
        /* $FlowFixMe(>=0.53.0 site=react_native_fb,react_native_oss) This
         * comment suppresses an error when upgrading Flow's support for
         * React. To see the error delete this comment and run Flow. */
        <RefreshControl
          refreshing={props.refreshing}
          onRefresh={props.onRefresh}
          progressViewOffset={props.progressViewOffset}
        />
      }
    />
  );
} else {
  return <ScrollView {...props} />;
}
```

不直接使用 Android 或 iOS 提供的列表组件呢

以 iOS 的 `UITableView` 为例，所有即将在视窗中呈现的元素都必须同步渲染。这意味着如果渲染过程超过 16ms，就会掉帧。

> In UITableView, when an element comes on screen, you have to synchronously render it. This means that you've got less than 16ms to do it. If you don't, then you drop one or multiple frames.

> 在UITableView中，当元素出现在屏幕上时，您必须同步渲染它。 这意味着你的时间不到16毫秒。 如果不这样做，则丢弃一个或多个帧。



但是问题是，从 RN render 到真正调用 native 代码这个过程本身是**异步**的，过程中消耗的时间也并不能保证在 16ms 以内。

> The problem is in the RN render -> shadow -> yoga -> native loop. You have at least three runloop jumps (dispatch_async(dispatch_get_main_queue(), ...) as well as background thread work, which all work against the required goal.

那么解决方案就是，在一些需要高性能的场景下，让 RN 能够**同步**的调用 native 代码。这个答案或许就是 ListView 性能问题的终极解决方案。

> We are actually starting to experiment more and more with synchronous method calls for other modules, which would allow us to build a native list component that could call `renderItem` on demand and choose whether to make the call synchronously on the UI thread if it's hi-pri (including the JS, react, and yoga/layout calcs), or on a background thread if it's a low-pri pre-render further off-screen. This native list component might also be able to do proper recycling and other optimizations.

> 我们实际上开始越来越多地尝试使用其他模块的同步方法调用，这将允许我们构建一个本地列表组件，可以根据需要调用`renderItem`并选择是否在UI线程上同步调用如果它是hi -pri（包括JS，反应和瑜伽/布局计算），或者在后台线程上，如果它是一个低pri预渲染进一步离屏。 此本机列表组件也可能能够进行适当的回收和其他优化。