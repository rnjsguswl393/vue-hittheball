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
	  
	<!-- <div>
      <vue-horizontal-list :items="items" :options="{responsive: [{end: 576, size: 1}, {size: 2}]}">
        <template v-slot:default="{item}">
          <div class="bg-steam border-3 overflow-hidden flex">
            <div class="ImageBoxed">
              <img :src="item.image"/>
            </div>

            <div class="p-16-24">
              <h5 class="text-ellipsis-1l">{{item.title}}</h5>
              <p class="text-ellipsis-2l">{{item.content}}</p>
            </div>
          </div>
        </template>
      </vue-horizontal-list>
    </div> -->
  </div>
	<!-- <div class="blog">
		<h1>{{title}}</h1>
		<div class="row">
			<div class="col-md-4">
				<div class="card">
					<div class="card-header">
						<h3>Vus.js</h3>
					</div>
					<div class="card-body">
						
						cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
						proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
					</div>
				</div>
			</div>
			<div class="col-md-4">
				<div class="card">
					<div class="card-header">
						<h3>Angular</h3>
					</div>
					<div class="card-body">
						
						cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
						proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
					</div>
				</div>
			</div>
			<div class="col-md-4">
				<div class="card">
					<div class="card-header">
						<h3>React</h3>
					</div>
					<div class="card-body">
						
						cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
						proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
					</div>
				</div>
			</div>
		</div>
	</div> -->
</template>
<script>

  import VueDaumMap from './VueDaumMap.vue';
  import config from './config';

  export default {
    name: 'App',
    components: {VueDaumMap},
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
        var marker=new kakao.maps.Marker({
		position: map.getCenter(), 
		clickable:true,
        map: map
	  	});
	  	var iwContent = '<div style="padding:5px;"><a href="http://localhost:8080/#/details/1" target="_blank"><strong>골프존파크</strong></a></div>', // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다
			iwRemoveable = true;
		var infowindow = new kakao.maps.InfoWindow({
    		content : iwContent,
    		removable : iwRemoveable
		});

		function test(){
			console.log("test");
		}

		kakao.maps.event.addListener(marker, 'click', function() {
      // 마커 위에 인포윈도우를 표시합니다
			infowindow.open(map, marker);  	  
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