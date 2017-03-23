# FuseSSP Android SDK

Maximize your app's revenue streams and save time, sign up for an account at [FuseSSP.com](http://fusessp.com).


## Initialization

#### Method:
```
init()
```
#### Parameters:
```
Context context//Context object.
String configUrl//Required, pease use this fixed address: http://adconfig.cpp.app.fusessp.net?pubid=xxxx, note that the value of pubid needs to be replaced by the "App ID" that was generated after the app you created in FuseSSP.
String channel
String installChannel
```
#### How to use:
```
AdAgent.getInstance().init(this, configUrl, " channel", "installChannel")
```

## Ad Data Obtainment
#### How to use:
```
public void loadAd(Context context, Ad ad, OnAdLoadListener listener)
AdAgent.getInstance().loadAd(getApplicationContext(),ad,new OnAdLoadListener() {
            @Override
            public void onLoad(IAd iAd) {
            }
            @Override
            public void onLoadFailed(AdError adError) {
            }
            @Override
            public void onLoadInterstitialAd
(WrapInterstitialAd wrapInterstitialAd) {                   
            }
        });
```
#### Method:
```
loadAd()
Ad ad = (new Ad.Builder(getApplicationContext(),slotId))//slotId is the "Ad Unit ID" generated after the ad unit you created in FuseSSP.
.setWidth(330)
.setHight(300)
.setParentViewGroup(ad_container)
.isPreLoad(false)
.setTransparent(false)
.setTitleColor(R.color.white)
.setSubTitleColor(R.color.white)
.setCtaTextColor(R.color.white)
.setCtaBackground(R.color.white)
.setAppSelfLayout(R.id.ad_resource_layout)
.build();
```
#### Parameters:
```
//Ad Builder:
ViewGroup viewGroup//Parent container (used to fill ads).
boolean isPreLoad//Control whether the ad is preloaded (default: false).
boolean setTransparent//Control whether the ad background is transparent (default: false).
int adResourceLayout//The layout id of the custom template layout in the xml.  
int titlecolor//The color of the title text in the ad element.
int subTitleColor//The color of the subtitle text in the ad element.
int ctaTextColor//The color of the CTA text in the ad element.
int ctaBackground//The background color of the CTA button in the ad element.
String slotId//Required, the "Ad Unit ID" that was generated after the app's ad unit you created in FuseSSP.
int width//The width of the ad (required in Admob banner ads)
int hight//The hight of the ad (required in Admob banner ads)
```
Additional information: ViewGroup viewGroup
Use getView() to get ad's view: don't provide the parent container, the ad returns Nativedata and adview by default, but the ad's data fill and display requires your application to complete. (Through the NativeData custom View need to use iAd.registerViewForInteraction (view) method to register the view in the onLoad callback process.)

Fill the parent container to dispaly the ads: provide the parent container, SDK will add the corresponding ad data you get to the corresponding parent class container, the data or view will also be provided in the callback interface onLoad (IAd iAd) method.

You can only fill the parent container to display the ads or use getView() to get ad's view, otherwise the click event may be invalid.

## Ad Data Callback
#### Method:
```
new OnAdLoadListener() {
                   @Override
                   public void onLoad(IAd iAd) {}
                   @Override
                   public void onLoadFailed(AdError adError) {}
                   @Override
                  public void onLoadInterstitialAd(WrapInterstitialAd wrapInterstitialAd) {
                                   wrapInterstitialAd.show();
                                             }}
```
#### onLoad: Native&Banner Ad
```
//IAd is the data returned after the callback is successful. Below are the methods in IAd.
View getAdView();//Banner view.
AdCallBackInfo getNativeAd();//Get native data.
String getIconUrl();//Get the url of the ad icon.
void setOnCancelAdListener(OnCancelAdListener var1);//Ad close button click event callback.
void setOnAdClickListener(OnAdClickListener var1);//Ad click event callback.
void setOnAdTouchListener(OnTouchListener var1);//Ad touch event callback.
nativeAd.getPrivacyInformationIconUrl()//Adchoice icon provided by networks.
voidsetOnPrivacyIconClickListener(OnClickListener var1, View var2);//The adchoice icon click event callback.
void registerViewForInteraction(View var1);//If you use the returned data to stitch View, you need to register the displayed View (capture data and trigger event response).
void showCustomAdView();//Custom view, you need to call this method to achieve the RBI event of the ad display.
```
#### onLoad: Interstitial Ad
```
wrapInterstitialAd.show()//Need to show manually.
```
#### onLoad: Failed 
```
AdError//Error information.
enum AdError { NO_FILL, NETWORK_FAILD, NETWORK_TIME_OUT, REQUEST_FREQUENCY, OTHER, INVALID_REQUEST;}
```

# Release Ad Resources
```
iAd.release(ad_container);//Required
ViewGroup ad_container//Load the ad parent control
```

# Customize Native UI
When you create an customize native ad object, the ad element id in the custom layout of the setAppSelfLayout() method needs to correspond to the ad element id in the table below.
|Ad Element | Required | Corresponding Ad Element ID |
|---|---|---|
| Ad icon | Required | R.id. icon_image_native |
| Cover image | Required | R.id.ad_cover_image |
| AD logo | Required | R.id.ad_icon_image2 |
| AdChoices icon | Required | R.id.native_ad_choices_image |
| call to action text | Yes | R.id.calltoaction_text |
| Title | Required | R.id.ad_title_text |
| Subtitle | Required | R.id.ad_subtitle_Text |
| Facebook mediaView |  | R.id.ad_fb_cover_image |
| Admob mediaView |  | R.id.ad_admob_cover_image |
