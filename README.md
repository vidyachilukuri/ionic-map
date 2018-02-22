If you’re running this on a device, make sure to install the Geolocation plugin by running:
$ionic plugin add cordova-plugin-geolocation
You will also need to install the Ionic Native package for this plugin with the following command:
$npm install --save @ionic-native/geolocation
src/index.html file
<script src="http://maps.google.com/maps/api/js?key=your_goolgle api key"></script>
<script src="cordova.js"></script>
map.html
<ion-header>
  <ion-navbar>
    <ion-title>
      Map
    </ion-title>
    <ion-buttons end>
      <button ion-button (click)="addMarker()"><ion-icon name="add"></ion-icon>Add Marker</button>
    </ion-buttons> 
  </ion-navbar>
</ion-header>
 
<ion-content>
  <div #map id="map"></div> 
</ion-content>
map.ts
import { Component,ViewChild, ElementRef  } from '@angular/core';
import { NavController } from 'ionic-angular';
import { Geolocation } from '@ionic-native/geolocation';
declare var google;
@Component({
  selector: 'page-map',
  templateUrl: 'map.html'
})
export class MapPage {

 @ViewChild('map') mapElement: ElementRef;
  map: any;
 z=0;
  constructor(public navCtrl: NavController, public geolocation: Geolocation) {
 
  }
 
  ionViewDidLoad(){
    this.loadMap();
  }
  
 
  loadMap(){
 for(var i=0;this.z<=i;i++){
    this.geolocation.getCurrentPosition().then((position) => {
 
      let latLng = new google.maps.LatLng(position.coords.latitude, position.coords.longitude);
 
      let mapOptions = {
        center: latLng,
        zoom: 15,
        mapTypeId: google.maps.MapTypeId.ROADMAP
      }
 
      this.map = new google.maps.Map(this.mapElement.nativeElement, mapOptions);
 let marker = new google.maps.Marker({
    map: this.map,
    animation: google.maps.Animation.DROP,
    position: this.map.getCenter()
  });
 
  let content = "<h4>Information!</h4>";         
 
  this.addInfoWindow(marker, content);
    

    }, (err) => {
      console.log(err);
    });
 }
  }
 addInfoWindow(marker, content){
 
  let infoWindow = new google.maps.InfoWindow({
    content: content
  });
 
  google.maps.event.addListener(marker, 'click', () => {
    infoWindow.open(this.map, marker);
  });
 
} 
  addMarker(){
  let marker = new google.maps.Marker({
    map: this.map,
    animation: google.maps.Animation.DROP,
    position: this.map.getCenter()
  });
 
  let content = "<h4>Information!</h4>";         
 
  this.addInfoWindow(marker, content);
 
}
 
  }
map.css
page-map {
 .scroll {
        height: 100%
      }
 
      #map {
        width: 100%;
        height: 100%;
      }
 
    }
    app.module.ts
    import { NgModule, ErrorHandler } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { IonicApp, IonicModule, IonicErrorHandler } from 'ionic-angular';
import { MyApp } from './app.component';
import { Geolocation } from '@ionic-native/geolocation';
import { AboutPage } from '../pages/about/about';
import { ContactPage } from '../pages/contact/contact';
import { HomePage } from '../pages/home/home';
import { TabsPage } from '../pages/tabs/tabs';

import { StatusBar } from '@ionic-native/status-bar';
import { SplashScreen } from '@ionic-native/splash-screen';

@NgModule({
  declarations: [
    MyApp,
    AboutPage,
    ContactPage,
    HomePage,
    TabsPage
  ],
  imports: [
    BrowserModule,
    IonicModule.forRoot(MyApp)
  ],
  bootstrap: [IonicApp],
  entryComponents: [
    MyApp,
    AboutPage,
    ContactPage,
    HomePage,
    TabsPage
  ],
  providers: [
    StatusBar,
    SplashScreen,
     Geolocation,
    {provide: ErrorHandler, useClass: IonicErrorHandler}
  ]
})
export class AppModule {}



then give ionic run android

