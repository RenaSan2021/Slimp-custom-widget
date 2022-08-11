# Slimp-custom-widget
Widget for Slimp app to use in streamelement overlay. Implements alert box functionality. Video in the end.

Here you can find code to recreate a custom widget in streamelement overlay. 
<img src="https://user-images.githubusercontent.com/79143038/184177385-37f85a2b-b411-402b-986d-18b53a161cb4.png" alt="Custom widget" width="700"/>

*Create a new custom widget*

<img src="https://user-images.githubusercontent.com/79143038/184185612-3d73cdc4-6e55-4c44-bb74-2b818d6c2058.png" alt="Open editor" width="700"/>

*Go to Settings --> Open editor*

<img src="https://user-images.githubusercontent.com/79143038/184186349-6d1997fc-df79-4c71-8b12-114088de1b3f.png" alt="Open editor tabs" width="700"/>

*You will see 5 tabs*

Just copy-past all the following parts in corresponding tabs.

# HTML

```html
<link href="https://fonts.googleapis.com/css?family=Montserrat:400,700" rel="stylesheet">

<div class="main-container">
<div class="text-container">
	<div class="image-container">
      <img id="realimg" src="" align="center">
  </div>
  <div class="awsome-text-container">
  </div>
</div>
</div>
```

# CSS

```css
@import url('https://cdn.streamelements.com/scripts/animate.min.css');


* {
    font-family: 'Montserrat', sans-serif;
    color: {{fontColor}};
    font-size: {{fontSize}}px;
    overflow: hidden;
}

.awsome-text-container {
  font-size: {{fontSize}}px;
    font-weight: bold;
}

.text-pop-container {
    font-size: {{fontSize}}px;
    font-weight: bold;
}

.text-plain-container {
    font-size: {{fontSize}}px;
    font-weight: bold;
  	color: rgb(255, 255, 255);
}

.animated-letter {
    animation-duration: 2s;
    animation-iteration-count: infinite;
    display: inline-block;
    animation-fill-mode: both;
    color: #3F51B5;
  	font-size: {{fontSizeName}}px;
}

.animated-letter:nth-child(1) {
    animation-delay: 0s;
}

.animated-letter:nth-child(2) {
    animation-delay: 0.1s;
}

.animated-letter:nth-child(3) {
    animation-delay: 0.2s;
}

.animated-letter:nth-child(4) {
    animation-delay: 0.3s;
}

.animated-letter:nth-child(5) {
    animation-delay: 0.4s;
}

.image-container {
    margin-top: {{defaultImgHeight}}%;
    display: table;
}

#realimg {
  	margin: auto;
  	width: {{defaultImageSize}}%; 
  	height: {{defaultImageSize}}%;
  	top: {{defaultImgHeight}}%;
  	z-index: 2;
  	display: block;
  	vertical-align: middle;
  	
}

.text-container {
    font-size: 16px;
    color: rgb(255, 255, 255);
    text-align: center;
    margin: auto;
    text-shadow: rgba(0, 0, 0, 0.8) 1px 1px 1px;
}

.username-container {
    font-size: {{fontSize}};
    color: rgb(255, 255, 255);
    text-align: center;
    margin: auto;
    text-shadow: rgba(0, 0, 0, 0.8) 1px 1px 1px;
}


.fadeOutClass{
    animation: fadeOut 1s;
    animation-delay:{{fadeoutTime}}s;
    animation-fill-mode: forwards;
}
@keyframes fadeOut {
    0% {
        opacity: 1;
    }
    100% {
        opacity: 0;
    }
}

@keyframes slide-in {
    0% {
        width: 0px;
    }
    100% {
        width: 100%;
    }
}

@keyframes pop-in {
    0% {
        width: 0px;
        height: 0px;
        background-size: 0px 0px;
        margin-top: 23px;
        margin-left: 24px;
    }
    50% {
        width: 0px;
        height: 0px;
        background-size: 0px 0px;
        margin-top: 12px;
        margin-left: 12px;
        margin-right: 12px;
        margin-bottom: 12px
    }
    85% {
        width: 23px;
        height: 23px;
        background-size: 23px 23px;
        margin-top: 1px;
        margin-left: 2px;
        margin-right: 0px;
        margin-bottom: 0px
    }
    100% {
        width: 21px;
        height: 21px;
        background-size: 21px 21px;
        margin-top: 2px;
        margin-left: 3px;
    }
}

@keyframes fade-in {
    0% {
        opacity: 0;
    }
    100% {
        opacity: 1;
    }
}

.event-container {
    width: 0px;
    height: 0px;

}

#image {
    z-index: 2;
  	position: absolute;
    width: {{defaultImageSize}}%; 
  height: {{defaultImageSize}}%;
  	top: {{defaultImgHeight}}%;
}
```

# JS

```js
let eventsLimit = 0,
    userLocale = "en-US",
    includeFollowers = true,
    includeRedemptions = true,
    includeHosts = true,
    minHost = 0,
    includeRaids = true,
    minRaid = 0,
    includeSubs = true,
    includeTips = true,
    minTip = 0,
    includeCheers = true,
    direction = "top",
    textOrder = "nameFirst",
    minCheer = 0;


let textFollower = "is now following",
    textRedeemed = "Redeemed",
    textSub = "new subscribed",
    textSubGift = "Sub gift",
	textHost = "is now hosting my stream with",
	textCheer = "Cheered",
	textRaid = "raided my stream with",
    textTip = "Tipped";

let defaultImageFollower = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageSub = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageRedeemed = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageSubGift = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageHost = "https://cdn.streamelements.com/static/alertbox/default.gif",  
    defaultImageCheer = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageRaid = "https://cdn.streamelements.com/static/alertbox/default.gif",
    defaultImageTip = "https://cdn.streamelements.com/static/alertbox/default.gif";
    

let userCurrency,
    totalEvents = 0;

let defaultImageSize = 25,
    defaultImgHeight = 1;




window.addEventListener('onEventReceived', function (obj) {
    if (!obj.detail.event) {
      return;
    }
    if (typeof obj.detail.event.itemId !== "undefined") {
        obj.detail.listener = "redemption-latest"
    }
    const listener = obj.detail.listener.split("-")[0];
    const event = obj.detail.event;


    if (listener === 'follower') {
        if (includeFollowers) {
            addEvent('follower','', textFollower, event.name, defaultImageFollower);
        }
    } else if (listener === 'redemption') {
        if (includeRedemptions) {
          addEvent('redemption','', textRedeemed, event.name, defaultImageRedeemed);
        }
    } else if (listener === 'subscriber') {
        if (includeSubs) {
            if (event.gifted) {
                addEvent('sub', `{textSubGift}`, `${event.amount}`, event.name, defaultImageSub);
            } else {
                addEvent('sub', '', textSub, event.name, defaultImageSubGift);
            }
        }
    } else if (listener === 'host') {
        if (includeHosts && minHost <= event.amount) {
            addEvent('host', `{textHost}`, `${event.amount.toLocaleString()} viewers`, event.name, defaultImageHost);
        }
    } else if (listener === 'cheer') {
        if (includeCheers && minCheer <= event.amount) {
            addEvent('cheer',`{textCheer}`, `${event.amount.toLocaleString()}`, event.name, defaultImageCheer);
        }
    } else if (listener === 'tip') {
        if (includeTips && minTip <= event.amount) {
            if (event.amount === parseInt(event.amount)) {
                addEvent('tip', `{textTip}`, `${event.amount.toLocaleString(userLocale, {
                    style: 'currency',
                    minimumFractionDigits: 0,
                    currency: userCurrency.code
                })}`, event.name, defaultImageTip);
            } else {
                addEvent('tip', '', `${event.amount.toLocaleString(userLocale, {
                    style: 'currency',
                    currency: userCurrency.code
                })}`, event.name, defaultImageTip);
            }
        }
    } else if (listener === 'raid') {
        if (includeRaids && minRaid <= event.amount) {
            addEvent('raid', `{textRaid}`, `${event.amount.toLocaleString()}`, event.name, defaultImageRaid);
        }
    }
});

window.addEventListener('onWidgetLoad', function (obj) {
    let recents = obj.detail.recents;
    recents.sort(function (a, b) {
        return Date.parse(a.createdAt) - Date.parse(b.createdAt);
    });
    userCurrency = obj.detail.currency;
    const fieldData = obj.detail.fieldData;
    includeFollowers = (fieldData.includeFollowers === "yes");
    includeRedemptions = (fieldData.includeRedemptions === "yes");
    includeHosts = (fieldData.includeHosts === "yes");
    minHost = fieldData.minHost;
    includeRaids = (fieldData.includeRaids === "yes");
    minRaid = fieldData.minRaid;
    includeSubs = (fieldData.includeSubs === "yes");
    includeTips = (fieldData.includeTips === "yes");
    minTip = fieldData.minTip;
    includeCheers = (fieldData.includeCheers === "yes");
    minCheer = fieldData.minCheer;
    direction = fieldData.direction;
    userLocale = fieldData.locale;
    textOrder = fieldData.textOrder;
    fadeoutTime = fieldData.fadeoutTime;
	textFollower = fieldData.textFollower;
    textRedeemed = fieldData.textRedeemed;
    textSubGift = fieldData.textSubGift;
  	textSub = fieldData.textSub;
	textHost = fieldData.textHost;
	textCheer = fieldData.textCheer;
	textRaid = fieldData.textRaid;
    textTip = fieldData.textTip;
  

    defaultImageFollower = fieldData.defaultImageFollower;
    defaultImageRedeemed = fieldData.defaultImageRedeemed;
    defaultImageSubGift = fieldData.defaultImageSubGift;
    defaultImageSub = fieldData.defaultImageSub;
    defaultImageHost = fieldData.defaultImageHost;
    defaultImageCheer = fieldData.defaultImageCheer;
    defaultImageRaid = fieldData.defaultImageRaid;
    defaultImageTip = fieldData.defaultImageTip;

  
  	//defaultImage = fieldData.defaultImage;
  	defaultImageSize = fieldData.defaultImageSize;
  	defaultImgHeight = fieldData.defaultImgHeight;

    let eventIndex;
    for (eventIndex = 0; eventIndex < recents.length; eventIndex++) {
        const event = recents[eventIndex];

        if (event.type === 'follower') {
            if (includeFollowers) {
                addEvent('follower', '', textFollower, event.name, defaultImageFollower);
            }
        } else if (event.type === 'redemption') {
            if (includeRedemptions) {
                addEvent('redemption', '', textRedeemed, event.name, defaultImageRedeemed);
            }
        } else if (event.type === 'subscriber') {
            if (!includeSubs) continue;
            if (event.amount === 'gift') {
                addEvent('sub', `{textSubGift}`, `${event.amount}`, event.name, defaultImageSub);
            } else {
                addEvent('sub', '', textSub, event.name, defaultImageSubGift);
            }

        } else if (event.type === 'host') {
            if (includeHosts && minHost <= event.amount) {
                addEvent('host', `{textHost}`, `${event.amount.toLocaleString()} viewers`, event.name, defaultImageHost);
            }
        } else if (event.type === 'cheer') {
            if (includeCheers && minCheer <= event.amount) {
                addEvent('cheer', `{textCheer}`, `${event.amount.toLocaleString()}`,  event.name, defaultImageCheer);
            }
        } else if (event.type === 'tip') {
            if (includeTips && minTip <= event.amount) {
                if (event.amount === parseInt(event.amount)) {
                    addEvent('tip', `{textTip}`, `${event.amount.toLocaleString(userLocale, {
                    style: 'currency',
                    minimumFractionDigits: 0,
                    currency: userCurrency.code
                })}`, event.name, defaultImageTip);
                } else {
                    addEvent('tip', '', `${event.amount.toLocaleString(userLocale, {
                        style: 'currency',
                        currency: userCurrency.code
                    })}`, event.name, defaultImageTip);
                }
            }
        } else if (event.type === 'raid') {
            if (includeRaids && minRaid <= event.amount) {
                addEvent('raid', `{textRaid}`, `${event.amount.toLocaleString()}`, event.name, defaultImageRaid);
            }
        }
    }
});


function addEvent(type, textplain, text,  username, image) {
  totalEvents += 1;
    let element,
        elementTwo;
  
	element = `<div class="event-container" id="event-${totalEvents}"></div>`;

    if (textOrder === "actionFirst") {
      elementTwo = `
			<span id="text-plain-container"></span>
			<span id="text-pop-container"></span>
         	<span id="username-container"></span>`;
    } else {
      elementTwo = `
			<span id="username-container"></span>
			<span id="text-plain-container"></span>
          	<span id="text-pop-container"></span>`;
    }
    $('.main-container').removeClass("fadeOutClass").show().append(element);
  	$('.awsome-text-container').removeClass("fadeOutClass").show().append(elementTwo);
  
    if (fadeoutTime !== 999) {
        $('.main-container').addClass("fadeOutClass");
    }
  
  	getName(`${username}`,`${text}`, `${textplain}`);
  	document.querySelector("img").setAttribute("src", `${image}`);
  	document.getElementById("image-container").style.margin = defaultImgHeight + "%";
	document.querySelector("img").style.height = defaultImageSize + "%";
  

 
}


function getName(name, text, textplain){
  const animation = 'wobble';
  const userNameContainer = document.querySelector('#username-container');
  const textpopContainer = document.querySelector('#text-pop-container');
  const textplainContainer = document.querySelector('#text-plain-container');
  userNameContainer.innerHTML = stringToAnimatedHTML(name, animation);
  textplainContainer.innerHTML = `<div class="text-plain-container">${textplain}</div>`;
  textpopContainer.innerHTML = `<div class="text-pop-container">${text}</div>`;

}

function stringToAnimatedHTML(s, anim) {
  	let stringAsArray = s.split('');
    stringAsArray = stringAsArray.map((letter) => {
        return `<span class="animated-letter ${anim}">${letter}</span>`
    });
    return stringAsArray.join('');
}
```

# Fields

```json
{
  "includeFollowers": {
    "type": "dropdown",
    "label": "Display followers:",
    "value": "yes",
    "group":"Follower",
    "options": {
      "yes": "Yes",
      "no": "No"
    }
  },
  "textFollower": {
    "type": "text",
    "label": "Text for followers:",
    "value": "is following",
    "group":"Follower"
  },
  "defaultImageFollower": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Follower"
  },
  "defaultImageSize": {
    "type": "slider",
    "label": "Image Size (%)",
    "value": 25,
    "min": 0,
    "max": 100,
    "steps": 1,
    "group":"Style"
  },
  "defaultImgHeight": {
    "type": "slider",
    "label": "Vertical pos. of the image (%)",
    "value": 1,
    "min": 1,
    "max": 100,
    "steps": 1,
    "group":"Style"
  },
  "includeRedemptions": {
    "type": "dropdown",
    "label": "Display redemptions:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Redeem"
  },
  "textRedeemed": {
    "type": "text",
    "label": "Text for Redeems:",
    "value": "Redeemed",
    "group":"Redeem"
  },
  "defaultImageRedeemed": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Redeem"
  },
  "includeHosts": {
    "type": "dropdown",
    "label": "Display hosts:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Host"
  },
  "textHost": {
    "type": "text",
    "label": "Text for Hosts",
    "value": "is now hosting my stream with"
    ,
    "group":"Host"
  },
  "minHost": {
    "type": "number",
    "label": "Minimum amount of hosting viewers:",
    "value": 1,
    "group":"Host"
  },
  "defaultImageHost": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Host"
  },
  "includeRaids": {
    "type": "dropdown",
    "label": "Display raids:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Raid"
  },
  "textRaid":{
    "type": "text",
    "label": "Text for Raid",
    "value": "raided my stream with",
    "group":"Raid"
  },
  "minRaid": {
    "type": "number",
    "label": "Minimum amount of raiding viewers:",
    "value": 1,
    "group":"Raid"
  },
  "defaultImageRaid": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Raid"
  },
  "includeSubs": {
    "type": "dropdown",
    "label": "Display subscribers:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Subscribers"
  },
    "textSubGift":{
    "type": "text",
    "label": "Text for SubGifs",
    "value": "gifted a subscribe",
    "group":"Subscribers"
  },
  	"textSub":{
    "type": "text",
    "label": "Text for Subs",
    "value": "just subscribed",
    "group":"Subscribers"
  },
  	"defaultImageSub": {
     "type": "image-input",
     "label": "Default Image for Sub",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Subscribers"
  },
  "defaultImageSubGift": {
     "type": "image-input",
     "label": "Default Image for SubGift",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Subscribers"
  },
  "includeTips": {
    "type": "dropdown",
    "label": "Display tips:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Tips"
  },
  "textTip":{
    "type": "text",
    "label": "Text for Tips",
    "value": "tipped",
    "group":"Tips"
  },
  "minTip": {
    "type": "number",
    "label": "Minimum amount of tip:",
    "value": 1,
    "group":"Tips"
  },
  "defaultImageTip": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Tips"
  },
  "includeCheers": {
    "type": "dropdown",
    "label": "Display cheers:",
    "value": "yes",
    "options": {
      "yes": "Yes",
      "no": "No"
    },
    "group":"Cheers"
  },
    "textCheer":{
    "type": "text",
    "label": "Text for Cheers",
    "value": "Cheered",
    "group":"Cheers"
  },
  "minCheer": {
    "type": "number",
    "label": "Minimum amount of bits in Cheer:",
    "value": 1,
    "group":"Cheers"
  },
  "defaultImageCheer": {
     "type": "image-input",
     "label": "Default Image",
    "value": "https://cdn.streamelements.com/static/alertbox/default.gif",
    "group":"Cheers"
  },
  "textOrder":{
    "type": "dropdown",
    "label": "Text order:",
    "value": "nameFirst",
    "options": {
      "nameFirst": "Name first",
      "actionFirst": "Action first"
    },
    "group":"Style"
  },
  "fadeoutTime": {
    "type": "number",
    "label": "Fadeout after action in seconds, 999 to disable:",
    "value": 999,
    "group":"Style"
  },
	"fontSize": {
    "type": "slider",
    "label": "Font Size (%)",
    "value": 14,
    "min": 0,
    "max": 100,
    "steps": 1,
    "group":"Style"
  },
	"fontSizeName": {
    "type": "slider",
    "label": "Name Font Size (%)",
    "value": 25,
    "min": 0,
    "max": 100,
    "steps": 1,
    "group":"Style"
  },
  "fontColor": {
    "type": "colorpicker",
    "label": "Font color",
    "value": "rgb(255, 255, 255)",
    "group":"Style"
  },
  "locale": {
    "type": "dropdown",
    "label": "Locale",
    "value": "en-US",
    "options": {
      "af-NA": "Afrikaans (Namibia)",
      "af-ZA": "Afrikaans (South Africa)",
      "af": "Afrikaans",
      "ak-GH": "Akan (Ghana)",
      "ak": "Akan",
      "sq-AL": "Albanian (Albania)",
      "sq": "Albanian",
      "am-ET": "Amharic (Ethiopia)",
      "am": "Amharic",
      "ar-DZ": "Arabic (Algeria)",
      "ar-BH": "Arabic (Bahrain)",
      "ar-EG": "Arabic (Egypt)",
      "ar-IQ": "Arabic (Iraq)",
      "ar-JO": "Arabic (Jordan)",
      "ar-KW": "Arabic (Kuwait)",
      "ar-LB": "Arabic (Lebanon)",
      "ar-LY": "Arabic (Libya)",
      "ar-MA": "Arabic (Morocco)",
      "ar-OM": "Arabic (Oman)",
      "ar-QA": "Arabic (Qatar)",
      "ar-SA": "Arabic (Saudi Arabia)",
      "ar-SD": "Arabic (Sudan)",
      "ar-SY": "Arabic (Syria)",
      "ar-TN": "Arabic (Tunisia)",
      "ar-AE": "Arabic (United Arab Emirates)",
      "ar-YE": "Arabic (Yemen)",
      "ar": "Arabic",
      "hy-AM": "Armenian (Armenia)",
      "hy": "Armenian",
      "as-IN": "Assamese (India)",
      "as": "Assamese",
      "asa-TZ": "Asu (Tanzania)",
      "asa": "Asu",
      "az-Cyrl": "Azerbaijani (Cyrillic)",
      "az-Cyrl-AZ": "Azerbaijani (Cyrillic, Azerbaijan)",
      "az-Latn": "Azerbaijani (Latin)",
      "az-Latn-AZ": "Azerbaijani (Latin, Azerbaijan)",
      "az": "Azerbaijani",
      "bm-ML": "Bambara (Mali)",
      "bm": "Bambara",
      "eu-ES": "Basque (Spain)",
      "eu": "Basque",
      "be-BY": "Belarusian (Belarus)",
      "be": "Belarusian",
      "bem-ZM": "Bemba (Zambia)",
      "bem": "Bemba",
      "bez-TZ": "Bena (Tanzania)",
      "bez": "Bena",
      "bn-BD": "Bengali (Bangladesh)",
      "bn-IN": "Bengali (India)",
      "bn": "Bengali",
      "bs-BA": "Bosnian (Bosnia and Herzegovina)",
      "bs": "Bosnian",
      "bg-BG": "Bulgarian (Bulgaria)",
      "bg": "Bulgarian",
      "my-MM": "Burmese (Myanmar [Burma])",
      "my": "Burmese",
      "ca-ES": "Catalan (Spain)",
      "ca": "Catalan",
      "tzm-Latn": "Central Morocco Tamazight (Latin)",
      "tzm-Latn-MA": "Central Morocco Tamazight (Latin, Morocco)",
      "tzm": "Central Morocco Tamazight",
      "chr-US": "Cherokee (United States)",
      "chr": "Cherokee",
      "cgg-UG": "Chiga (Uganda)",
      "cgg": "Chiga",
      "zh-Hans": "Chinese (Simplified Han)",
      "zh-Hans-CN": "Chinese (Simplified Han, China)",
      "zh-Hans-HK": "Chinese (Simplified Han, Hong Kong SAR China)",
      "zh-Hans-MO": "Chinese (Simplified Han, Macau SAR China)",
      "zh-Hans-SG": "Chinese (Simplified Han, Singapore)",
      "zh-Hant": "Chinese (Traditional Han)",
      "zh-Hant-HK": "Chinese (Traditional Han, Hong Kong SAR China)",
      "zh-Hant-MO": "Chinese (Traditional Han, Macau SAR China)",
      "zh-Hant-TW": "Chinese (Traditional Han, Taiwan)",
      "zh": "Chinese",
      "kw-GB": "Cornish (United Kingdom)",
      "kw": "Cornish",
      "hr-HR": "Croatian (Croatia)",
      "hr": "Croatian",
      "cs-CZ": "Czech (Czech Republic)",
      "cs": "Czech",
      "da-DK": "Danish (Denmark)",
      "da": "Danish",
      "nl-BE": "Dutch (Belgium)",
      "nl-NL": "Dutch (Netherlands)",
      "nl": "Dutch",
      "ebu-KE": "Embu (Kenya)",
      "ebu": "Embu",
      "en-AS": "English (American Samoa)",
      "en-AU": "English (Australia)",
      "en-BE": "English (Belgium)",
      "en-BZ": "English (Belize)",
      "en-BW": "English (Botswana)",
      "en-CA": "English (Canada)",
      "en-GU": "English (Guam)",
      "en-HK": "English (Hong Kong SAR China)",
      "en-IN": "English (India)",
      "en-IE": "English (Ireland)",
      "en-JM": "English (Jamaica)",
      "en-MT": "English (Malta)",
      "en-MH": "English (Marshall Islands)",
      "en-MU": "English (Mauritius)",
      "en-NA": "English (Namibia)",
      "en-NZ": "English (New Zealand)",
      "en-MP": "English (Northern Mariana Islands)",
      "en-PK": "English (Pakistan)",
      "en-PH": "English (Philippines)",
      "en-SG": "English (Singapore)",
      "en-ZA": "English (South Africa)",
      "en-TT": "English (Trinidad and Tobago)",
      "en-UM": "English (U.S. Minor Outlying Islands)",
      "en-VI": "English (U.S. Virgin Islands)",
      "en-GB": "English (United Kingdom)",
      "en-US": "English (United States)",
      "en-ZW": "English (Zimbabwe)",
      "en": "English",
      "eo": "Esperanto",
      "et-EE": "Estonian (Estonia)",
      "et": "Estonian",
      "ee-GH": "Ewe (Ghana)",
      "ee-TG": "Ewe (Togo)",
      "ee": "Ewe",
      "fo-FO": "Faroese (Faroe Islands)",
      "fo": "Faroese",
      "fil-PH": "Filipino (Philippines)",
      "fil": "Filipino",
      "fi-FI": "Finnish (Finland)",
      "fi": "Finnish",
      "fr-BE": "French (Belgium)",
      "fr-BJ": "French (Benin)",
      "fr-BF": "French (Burkina Faso)",
      "fr-BI": "French (Burundi)",
      "fr-CM": "French (Cameroon)",
      "fr-CA": "French (Canada)",
      "fr-CF": "French (Central African Republic)",
      "fr-TD": "French (Chad)",
      "fr-KM": "French (Comoros)",
      "fr-CG": "French (Congo - Brazzaville)",
      "fr-CD": "French (Congo - Kinshasa)",
      "fr-CI": "French (Côte d’Ivoire)",
      "fr-DJ": "French (Djibouti)",
      "fr-GQ": "French (Equatorial Guinea)",
      "fr-FR": "French (France)",
      "fr-GA": "French (Gabon)",
      "fr-GP": "French (Guadeloupe)",
      "fr-GN": "French (Guinea)",
      "fr-LU": "French (Luxembourg)",
      "fr-MG": "French (Madagascar)",
      "fr-ML": "French (Mali)",
      "fr-MQ": "French (Martinique)",
      "fr-MC": "French (Monaco)",
      "fr-NE": "French (Niger)",
      "fr-RW": "French (Rwanda)",
      "fr-RE": "French (Réunion)",
      "fr-BL": "French (Saint Barthélemy)",
      "fr-MF": "French (Saint Martin)",
      "fr-SN": "French (Senegal)",
      "fr-CH": "French (Switzerland)",
      "fr-TG": "French (Togo)",
      "fr": "French",
      "ff-SN": "Fulah (Senegal)",
      "ff": "Fulah",
      "gl-ES": "Galician (Spain)",
      "gl": "Galician",
      "lg-UG": "Ganda (Uganda)",
      "lg": "Ganda",
      "ka-GE": "Georgian (Georgia)",
      "ka": "Georgian",
      "de-AT": "German (Austria)",
      "de-BE": "German (Belgium)",
      "de-DE": "German (Germany)",
      "de-LI": "German (Liechtenstein)",
      "de-LU": "German (Luxembourg)",
      "de-CH": "German (Switzerland)",
      "de": "German",
      "el-CY": "Greek (Cyprus)",
      "el-GR": "Greek (Greece)",
      "el": "Greek",
      "gu-IN": "Gujarati (India)",
      "gu": "Gujarati",
      "guz-KE": "Gusii (Kenya)",
      "guz": "Gusii",
      "ha-Latn": "Hausa (Latin)",
      "ha-Latn-GH": "Hausa (Latin, Ghana)",
      "ha-Latn-NE": "Hausa (Latin, Niger)",
      "ha-Latn-NG": "Hausa (Latin, Nigeria)",
      "ha": "Hausa",
      "haw-US": "Hawaiian (United States)",
      "haw": "Hawaiian",
      "he-IL": "Hebrew (Israel)",
      "he": "Hebrew",
      "hi-IN": "Hindi (India)",
      "hi": "Hindi",
      "hu-HU": "Hungarian (Hungary)",
      "hu": "Hungarian",
      "is-IS": "Icelandic (Iceland)",
      "is": "Icelandic",
      "ig-NG": "Igbo (Nigeria)",
      "ig": "Igbo",
      "id-ID": "Indonesian (Indonesia)",
      "id": "Indonesian",
      "ga-IE": "Irish (Ireland)",
      "ga": "Irish",
      "it-IT": "Italian (Italy)",
      "it-CH": "Italian (Switzerland)",
      "it": "Italian",
      "ja-JP": "Japanese (Japan)",
      "ja": "Japanese",
      "kea-CV": "Kabuverdianu (Cape Verde)",
      "kea": "Kabuverdianu",
      "kab-DZ": "Kabyle (Algeria)",
      "kab": "Kabyle",
      "kl-GL": "Kalaallisut (Greenland)",
      "kl": "Kalaallisut",
      "kln-KE": "Kalenjin (Kenya)",
      "kln": "Kalenjin",
      "kam-KE": "Kamba (Kenya)",
      "kam": "Kamba",
      "kn-IN": "Kannada (India)",
      "kn": "Kannada",
      "kk-Cyrl": "Kazakh (Cyrillic)",
      "kk-Cyrl-KZ": "Kazakh (Cyrillic, Kazakhstan)",
      "kk": "Kazakh",
      "km-KH": "Khmer (Cambodia)",
      "km": "Khmer",
      "ki-KE": "Kikuyu (Kenya)",
      "ki": "Kikuyu",
      "rw-RW": "Kinyarwanda (Rwanda)",
      "rw": "Kinyarwanda",
      "kok-IN": "Konkani (India)",
      "kok": "Konkani",
      "ko-KR": "Korean (South Korea)",
      "ko": "Korean",
      "khq-ML": "Koyra Chiini (Mali)",
      "khq": "Koyra Chiini",
      "ses-ML": "Koyraboro Senni (Mali)",
      "ses": "Koyraboro Senni",
      "lag-TZ": "Langi (Tanzania)",
      "lag": "Langi",
      "lv-LV": "Latvian (Latvia)",
      "lv": "Latvian",
      "lt-LT": "Lithuanian (Lithuania)",
      "lt": "Lithuanian",
      "luo-KE": "Luo (Kenya)",
      "luo": "Luo",
      "luy-KE": "Luyia (Kenya)",
      "luy": "Luyia",
      "mk-MK": "Macedonian (Macedonia)",
      "mk": "Macedonian",
      "jmc-TZ": "Machame (Tanzania)",
      "jmc": "Machame",
      "kde-TZ": "Makonde (Tanzania)",
      "kde": "Makonde",
      "mg-MG": "Malagasy (Madagascar)",
      "mg": "Malagasy",
      "ms-BN": "Malay (Brunei)",
      "ms-MY": "Malay (Malaysia)",
      "ms": "Malay",
      "ml-IN": "Malayalam (India)",
      "ml": "Malayalam",
      "mt-MT": "Maltese (Malta)",
      "mt": "Maltese",
      "gv-GB": "Manx (United Kingdom)",
      "gv": "Manx",
      "mr-IN": "Marathi (India)",
      "mr": "Marathi",
      "mas-KE": "Masai (Kenya)",
      "mas-TZ": "Masai (Tanzania)",
      "mas": "Masai",
      "mer-KE": "Meru (Kenya)",
      "mer": "Meru",
      "mfe-MU": "Morisyen (Mauritius)",
      "mfe": "Morisyen",
      "naq-NA": "Nama (Namibia)",
      "naq": "Nama",
      "ne-IN": "Nepali (India)",
      "ne-NP": "Nepali (Nepal)",
      "ne": "Nepali",
      "nd-ZW": "North Ndebele (Zimbabwe)",
      "nd": "North Ndebele",
      "nb-NO": "Norwegian Bokmål (Norway)",
      "nb": "Norwegian Bokmål",
      "nn-NO": "Norwegian Nynorsk (Norway)",
      "nn": "Norwegian Nynorsk",
      "nyn-UG": "Nyankole (Uganda)",
      "nyn": "Nyankole",
      "or-IN": "Oriya (India)",
      "or": "Oriya",
      "om-ET": "Oromo (Ethiopia)",
      "om-KE": "Oromo (Kenya)",
      "om": "Oromo",
      "ps-AF": "Pashto (Afghanistan)",
      "ps": "Pashto",
      "fa-AF": "Persian (Afghanistan)",
      "fa-IR": "Persian (Iran)",
      "fa": "Persian",
      "pl-PL": "Polish (Poland)",
      "pl": "Polish",
      "pt-BR": "Portuguese (Brazil)",
      "pt-GW": "Portuguese (Guinea-Bissau)",
      "pt-MZ": "Portuguese (Mozambique)",
      "pt-PT": "Portuguese (Portugal)",
      "pt": "Portuguese",
      "pa-Arab": "Punjabi (Arabic)",
      "pa-Arab-PK": "Punjabi (Arabic, Pakistan)",
      "pa-Guru": "Punjabi (Gurmukhi)",
      "pa-Guru-IN": "Punjabi (Gurmukhi, India)",
      "pa": "Punjabi",
      "ro-MD": "Romanian (Moldova)",
      "ro-RO": "Romanian (Romania)",
      "ro": "Romanian",
      "rm-CH": "Romansh (Switzerland)",
      "rm": "Romansh",
      "rof-TZ": "Rombo (Tanzania)",
      "rof": "Rombo",
      "ru-MD": "Russian (Moldova)",
      "ru-RU": "Russian (Russia)",
      "ru-UA": "Russian (Ukraine)",
      "ru": "Russian",
      "rwk-TZ": "Rwa (Tanzania)",
      "rwk": "Rwa",
      "saq-KE": "Samburu (Kenya)",
      "saq": "Samburu",
      "sg-CF": "Sango (Central African Republic)",
      "sg": "Sango",
      "seh-MZ": "Sena (Mozambique)",
      "seh": "Sena",
      "sr-Cyrl": "Serbian (Cyrillic)",
      "sr-Cyrl-BA": "Serbian (Cyrillic, Bosnia and Herzegovina)",
      "sr-Cyrl-ME": "Serbian (Cyrillic, Montenegro)",
      "sr-Cyrl-RS": "Serbian (Cyrillic, Serbia)",
      "sr-Latn": "Serbian (Latin)",
      "sr-Latn-BA": "Serbian (Latin, Bosnia and Herzegovina)",
      "sr-Latn-ME": "Serbian (Latin, Montenegro)",
      "sr-Latn-RS": "Serbian (Latin, Serbia)",
      "sr": "Serbian",
      "sn-ZW": "Shona (Zimbabwe)",
      "sn": "Shona",
      "ii-CN": "Sichuan Yi (China)",
      "ii": "Sichuan Yi",
      "si-LK": "Sinhala (Sri Lanka)",
      "si": "Sinhala",
      "sk-SK": "Slovak (Slovakia)",
      "sk": "Slovak",
      "sl-SI": "Slovenian (Slovenia)",
      "sl": "Slovenian",
      "xog-UG": "Soga (Uganda)",
      "xog": "Soga",
      "so-DJ": "Somali (Djibouti)",
      "so-ET": "Somali (Ethiopia)",
      "so-KE": "Somali (Kenya)",
      "so-SO": "Somali (Somalia)",
      "so": "Somali",
      "es-AR": "Spanish (Argentina)",
      "es-BO": "Spanish (Bolivia)",
      "es-CL": "Spanish (Chile)",
      "es-CO": "Spanish (Colombia)",
      "es-CR": "Spanish (Costa Rica)",
      "es-DO": "Spanish (Dominican Republic)",
      "es-EC": "Spanish (Ecuador)",
      "es-SV": "Spanish (El Salvador)",
      "es-GQ": "Spanish (Equatorial Guinea)",
      "es-GT": "Spanish (Guatemala)",
      "es-HN": "Spanish (Honduras)",
      "es-419": "Spanish (Latin America)",
      "es-MX": "Spanish (Mexico)",
      "es-NI": "Spanish (Nicaragua)",
      "es-PA": "Spanish (Panama)",
      "es-PY": "Spanish (Paraguay)",
      "es-PE": "Spanish (Peru)",
      "es-PR": "Spanish (Puerto Rico)",
      "es-ES": "Spanish (Spain)",
      "es-US": "Spanish (United States)",
      "es-UY": "Spanish (Uruguay)",
      "es-VE": "Spanish (Venezuela)",
      "es": "Spanish",
      "sw-KE": "Swahili (Kenya)",
      "sw-TZ": "Swahili (Tanzania)",
      "sw": "Swahili",
      "sv-FI": "Swedish (Finland)",
      "sv-SE": "Swedish (Sweden)",
      "sv": "Swedish",
      "gsw-CH": "Swiss German (Switzerland)",
      "gsw": "Swiss German",
      "shi-Latn": "Tachelhit (Latin)",
      "shi-Latn-MA": "Tachelhit (Latin, Morocco)",
      "shi-Tfng": "Tachelhit (Tifinagh)",
      "shi-Tfng-MA": "Tachelhit (Tifinagh, Morocco)",
      "shi": "Tachelhit",
      "dav-KE": "Taita (Kenya)",
      "dav": "Taita",
      "ta-IN": "Tamil (India)",
      "ta-LK": "Tamil (Sri Lanka)",
      "ta": "Tamil",
      "te-IN": "Telugu (India)",
      "te": "Telugu",
      "teo-KE": "Teso (Kenya)",
      "teo-UG": "Teso (Uganda)",
      "teo": "Teso",
      "th-TH": "Thai (Thailand)",
      "th": "Thai",
      "bo-CN": "Tibetan (China)",
      "bo-IN": "Tibetan (India)",
      "bo": "Tibetan",
      "ti-ER": "Tigrinya (Eritrea)",
      "ti-ET": "Tigrinya (Ethiopia)",
      "ti": "Tigrinya",
      "to-TO": "Tonga (Tonga)",
      "to": "Tonga",
      "tr-TR": "Turkish (Turkey)",
      "tr": "Turkish",
      "uk-UA": "Ukrainian (Ukraine)",
      "uk": "Ukrainian",
      "ur-IN": "Urdu (India)",
      "ur-PK": "Urdu (Pakistan)",
      "ur": "Urdu",
      "uz-Arab": "Uzbek (Arabic)",
      "uz-Arab-AF": "Uzbek (Arabic, Afghanistan)",
      "uz-Cyrl": "Uzbek (Cyrillic)",
      "uz-Cyrl-UZ": "Uzbek (Cyrillic, Uzbekistan)",
      "uz-Latn": "Uzbek (Latin)",
      "uz-Latn-UZ": "Uzbek (Latin, Uzbekistan)",
      "uz": "Uzbek",
      "vi-VN": "Vietnamese (Vietnam)",
      "vi": "Vietnamese",
      "vun-TZ": "Vunjo (Tanzania)",
      "vun": "Vunjo",
      "cy-GB": "Welsh (United Kingdom)",
      "cy": "Welsh",
      "yo-NG": "Yoruba (Nigeria)",
      "yo": "Yoruba",
      "zu-ZA": "Zulu (South Africa)",
      "zu": "Zulu"
    },
    "group":"Style"
  }
}
```

# Data

```json
{"eventsLimit":0,"includeFollowers":"yes","includeRedemptions":"yes","includeHosts":"yes","minHost":1,"includeRaids":"yes","minRaid":1,"includeSubs":"yes","includeTips":"yes","minTip":1,"includeCheers":"yes","minCheer":1,"direction":"top","textOrder":"nameFirst","fadeoutTime":5,"fontColor":"rgb(91, 207, 90)","theme":"hexagons","locale":"en-US","textFollower":"is following!","textRedeemed":"Redeemed","textHost":"is now hosting my stream with","textRaid":"Raid","textSubGift":"Gift subsribtion","textCheer":"Cheered x","textSub":"just subscribed!","textTip":"tipped","defaultImageSize":46,"defaultImgHeight":1,"defaultImageFollower":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageSizeFollower":89,"defaultImgHeightFollower":22,"defaultImageRedeemed":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageRaid":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageHost":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageSub":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageSubGift":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageTip":"https://cdn.streamelements.com/static/alertbox/default.gif","defaultImageCheer":"https://cdn.streamelements.com/static/alertbox/default.gif","fontSize":20,"fontSizeName":29}
```

# That's it

