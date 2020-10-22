<template>
  <div>
    <h1>카카오 지도</h1>

    <vue-daum-map 

      :appKey="appKey"

      :center.sync="center"
      :level.sync="level"
      :mapTypeId="mapTypeId"
      :libraries="libraries"

      @load="onLoad"

      @center_changed="onMapEvent('center_changed', $event)"
      @zoom_start="onMapEvent('zoom_start', $event)"
      @zoom_changed="onMapEvent('zoom_changed', $event)"
      @bounds_changed="onMapEvent('bounds_changed', $event)"
      @click="onMapEvent('click', $event)"
      @dblclick="onMapEvent('dblclick', $event)"
      @rightclick="onMapEvent('rightclick', $event)"
      @mousemove="onMapEvent('mousemove', $event)"
      @dragstart="onMapEvent('dragstart', $event)"
      @drag="onMapEvent('drag', $event)"
      @dragend="onMapEvent('dragend', $event)"
      @idle="onMapEvent('idle', $event)"
      @tilesloaded="onMapEvent('tilesloaded', $event)"
      @maptypeid_changed="onMapEvent('maptypeid_changed', $event)"

      style="width:350px;height:350px; margin:0 auto">
    </vue-daum-map>

    <h2>상세 경위도</h2>
    <table>
      <colgroup>
        <col width="60" />
        <col />
      </colgroup>
      <tr>
        <th>레벨</th>
        <td><input type="range" min="1" max="14" v-model.number="level" > {{level}}</td>
      </tr>
      <tr>
        <th>경도</th>
        <td><input type="number" v-model.number="center.lat" step="0.0001"></td>
      </tr>
      <tr>
        <th>위도</th>
        <td><input type="number" v-model.number="center.lng" step="0.0001"></td>
      </tr>
    </table>
	<div id="app"></div>

    <!-- <div class="vhl-container" :style="_style.container">
      <div class="vhl-list" ref="list" :class="_options.list.class" :style="_style.list">
        <div v-for="item in items" ref="item" class="vhl-item" :class="_options.item.class" :style="_style.item">
          <slot v-bind:item="item">{{item}}</slot>
        </div>

        <div :style="_style.tail">
        </div>
      </div>
    </div> -->
  <a href="http://localhost:8080/#/details/1"> <p>여기에 div추가</p></a>
  </div>
	
</template>
<script>
  import golf from '../assets/data/연습장.json'
  import VueDaumMap from './VueDaumMap.vue';
  import config from './config';

  export default {
    name: 'App',
    components: {VueDaumMap},
    // props:{
    //   items: {
    //     type: Array,
    //     required: true
    //   },
    //   options: {
    //     type: Object,
    //     required: false
    //   }
    // },
    data: () => ({
      appKey: config.appKey,
      center: {lat:37.523291, lng:127.0545508},
      level: 3,
      mapTypeId: VueDaumMap.MapTypeId.NORMAL,
      libraries: ["services","clusterer","drawing"],
      mapObject: null,
      // marker: {lat:37.523191, lng:127.0534508},
    }),
    methods: {
      onLoad (map) {
        // 지도의 현재 영역을 얻어옵니다
        var bounds = map.getBounds();

        // 영역정보를 문자열로 얻어옵니다. ((남,서), (북,동)) 형식입니다
        var boundsStr = bounds.toString();

        console.log('Daum Map Loaded', boundsStr);

		this.mapObject = map;
		
        //마커마커마컼마컼ㅋ
    //   var marker=new kakao.maps.Marker({
		//     position: map.getCenter(), 
		//     clickable:true,
    //     map: map
	  // 	});
	  // 	var iwContent = '<div style="padding:5px;"><a href="http://localhost:8080/#/details/1" target="_blank"><strong>골프존파크</strong></a></div>', // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다
		// 	iwRemoveable = true;
		//   var infowindow = new kakao.maps.InfoWindow({
    // 		content : iwContent,
    // 		removable : iwRemoveable
    // });
    var positions=new Array;
    // console.log(Object.values(golf));
    var content=new Array;
    golf.golfplace.forEach((item)=>{
      positions.push(new kakao.maps.LatLng(item.latitude,item.longitude));
      content.push(item.name);
    });
    console.log(positions);

    console.log(content);
//     var positions = [
//     {
//         content: '<div>카카오</div>', 
//         latlng: new kakao.maps.LatLng(33.450705, 126.570677)
//     },
//     {
//         content: '<div>생태연못</div>', 
//         latlng: new kakao.maps.LatLng(33.450936, 126.569477)
//     },
//     {
//         content: '<div>텃밭</div>', 
//         latlng: new kakao.maps.LatLng(33.450879, 126.569940)
//     },
//     {
//         content: '<div>근린공원</div>',
//         latlng: new kakao.maps.LatLng(33.451393, 126.570738)
//     }
// ];
var clusterer = new kakao.maps.MarkerClusterer({
        map: map, // 마커들을 클러스터로 관리하고 표시할 지도 객체 
        averageCenter: true, // 클러스터에 포함된 마커들의 평균 위치를 클러스터 마커 위치로 설정 
        minLevel: 10 // 클러스터 할 최소 지도 레벨 
    });
    for (var i = 0; i < positions.length; i ++) {
    // 마커를 생성합니다
    var markers = new kakao.maps.Marker({
        map: map, // 마커를 표시할 지도
        position: positions[i] // 마커의 위치
    });
    clusterer.addMarkers(markers);
    // 마커에 표시할 인포윈도우를 생성합니다 
    var infowindow = new kakao.maps.InfoWindow({
        content: content[i] // 인포윈도우에 표시할 내용
    });

    // 마커에 mouseover 이벤트와 mouseout 이벤트를 등록합니다
    // 이벤트 리스너로는 클로저를 만들어 등록합니다 
    // for문에서 클로저를 만들어 주지 않으면 마지막 마커에만 이벤트가 등록됩니다
    kakao.maps.event.addListener(markers, 'mouseover', makeOverListener(map, markers, infowindow));
    kakao.maps.event.addListener(markers, 'mouseout', makeOutListener(infowindow));
    }
    // 인포윈도우를 표시하는 클로저를 만드는 함수입니다 
function makeOverListener(map, markers, infowindow) {
    return function() {
        infowindow.open(map, markers);
    };
}

// 인포윈도우를 닫는 클로저를 만드는 함수입니다 
function makeOutListener(infowindow) {
    return function() {
        infowindow.close();
    };
}

		kakao.maps.event.addListener(markers, 'click', function() {
      // 마커 위에 인포윈도우를 표시합니다
			infowindow.open(map, markers);  	  
		});
  


		//현재위치마커마커
		if (navigator.geolocation) {
    
    // GeoLocation을 이용해서 접속 위치를 얻어옵니다
    		navigator.geolocation.getCurrentPosition(function(position) {
        
       	 		var lat = position.coords.latitude, // 위도
            		lon = position.coords.longitude; // 경도
        
        		var locPosition = new kakao.maps.LatLng(lat, lon), // 마커가 표시될 위치를 geolocation으로 얻어온 좌표로 생성합니다
            		message = '<div style="padding:5px;">현재위치입니다</div>'; // 인포윈도우에 표시될 내용입니다
        
        // 마커와 인포윈도우를 표시합니다
        		displayMarker(locPosition, message);
				
      		});
    
	} else { // HTML5의 GeoLocation을 사용할 수 없을때 마커 표시 위치와 인포윈도우 내용을 설정합니다
    
    	var locPosition = new kakao.maps.LatLng(37.523291,127.0545508),    
        	message = 'geolocation을 사용할수 없어요..'
        
    	displayMarker(locPosition, message);
	}

// 지도에 마커와 인포윈도우를 표시하는 함수입니다
	function displayMarker(locPosition, message) {

    // 마커를 생성합니다
    	var marker = new kakao.maps.Marker({  
        	map: map, 
        	position: locPosition
    	}); 
    
    	var iwContent = message, // 인포윈도우에 표시할 내용
        	iwRemoveable = true;

    // 인포윈도우를 생성합니다
    	var infowindow = new kakao.maps.InfoWindow({
       		content : iwContent,
        	removable : iwRemoveable
    	});
    
    // 인포윈도우를 마커위에 표시합니다 
    	infowindow.open(map, marker);
    
    // 지도 중심좌표를 접속위치로 변경합니다
    	map.setCenter(locPosition);      
    } 
    
	},
	  
      onMapEvent (event, params) {
        console.log(`Daum Map Event(${event})`, params);
        
      }
    }
  }
	// export default{
	// 	name:'blog',
	// 	data (){
	// 		return{
	// 			title:'Blog'
	// 		}
	// 	},
		
	// }
</script>

<style scoped>
	
</style>