# 高德地图
高德地图，接触到的知识点，待扩充

#### 生成地图
```
var map = new AMap.Map('container', {resizeEnable: true});
```

#### 添加toolbar工具
```
AMap.plugin(['AMap.ToolBar'], function(){
    toolopt = {
        offset :new AMap.Pixel(10,10),
        position : 'RT',
        direction : false,
        locationMarker : AMap.Marker({map: map}),
        useNative : false
    }
    var toolbar  = new AMap.ToolBar(toolopt);
    map.addControl(toolbar);
});
```

#### 地理编码
```
var geocoder,marker;
function geoCode() {
    if(!geocoder){
        geocoder = new AMap.Geocoder({
            city: "全国",
        });
    }
    var address  = '北京市朝阳区阜荣街10号';
    geocoder.getLocation(address, function(status, result) {
        if (status === 'complete'&&result.geocodes.length) {
            var lnglat = result.geocodes[0].location
            if(!marker){
                marker = new AMap.Marker();
                map.add(marker);
            }
            marker.setPosition(lnglat);
            map.setFitView(marker);
        }else{alert(JSON.stringify(result))}
    });
}
```

#### 点聚合+创建标记+信息窗体
```
var points =[
        {position: [122.102956,37.099001], icon: 'asset/images/icon_03.png'},
        {position: [122.368595,37.41136], icon: 'asset/images/icon_02.png'},
        {position: [122.569482,37.166254], icon: 'asset/images/icon_03.png'},
        {position: [122.430267,36.882894], icon: 'asset/images/icon_02.png'},
        {position: [122.159309,37.432808], icon: 'asset/images/icon_03.png'},
        {position: [122.123711,37.509635], icon: 'asset/images/icon_02.png'},
        {position: [122.120838,37.504814], icon: 'asset/images/icon_03.png'},
        {position: [122.697427,37.401789], icon: 'asset/images/icon_02.png'},
        {position: [122.160324,37.448521], icon: 'asset/images/icon_03.png'},
        {position: [122.092744,37.020144], icon: 'asset/images/icon_02.png'}
    ], markers = [];

var infoWindow = new AMap.InfoWindow({offset: new AMap.Pixel(0, -30)});
for(var i=0; i< points.length; i++){
    var marker = new AMap.Marker({
        position: points[i].position,
        icon: points[i].icon,
        map: map
    });
    marker.content = '<div class="msgwindow">'+
                        '<div class="line">차량번호 : 34가8381</div>'+
                        '<div class="line">상태 : 운행종료</div>'+
                        '<div class="line">차량모델 : 오딧세이</div>'+
                        '<div class="line">현재위치 : 서울특별시 송파구 가락본동 59</div>'+
                        '<div class="line-button"><button class="currentmsg">-</button></div>';
                    '</div>';

    marker.on('click', markerClick);
    // marker.emit('click', {target: marker});

    markers.push(marker);
}
function markerClick(e) {
    infoWindow.setContent(e.target.content);
    infoWindow.open(map, e.target.getPosition());
}
map.setFitView();

/*var styles = [{  // 自定义聚合后的图片
    url: "asset/images/group.png",
    size: new AMap.Size(32,32)
}];*/

// 点聚合
AMap.plugin(["AMap.MarkerClusterer"],function() {
    cluster = new AMap.MarkerClusterer(map, markers);

    // 地图信息窗体
    // markers.forEach(function(item, index, array){
    //     AMapUI.loadUI(['overlay/SimpleInfoWindow'], function(SimpleInfoWindow) {
    //         var infoWindow = new SimpleInfoWindow({
    //             // infoTitle: '<strong>这里是标题</strong>',
    //             infoBody: '<p class="my-desc"><strong>这里是内容。</strong> <br/> 高德地图 JavaScript API，是由 JavaScript 语言编写的应用程序接口，' +
    //                 '它能够帮助您在网站或移动端中构建功能丰富、交互性强的地图应用程序</p>',
    //             //基点指向marker的头部位置
    //             offset: new AMap.Pixel(0, -31)
    //         });
    //         //marker 点击时打开
    //         AMap.event.addListener(item, 'click', function() {
    //             openInfoWin();
    //         });
    //         function openInfoWin() {
    //             infoWindow.open(map, item.getPosition());
    //         }
    //     });
    // })
});
```
