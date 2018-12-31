# LivescoreFeed.net XML API docs

Official docs of the live sport data XML API of LivescoreFeed.net

## Building URLs with custom GET parameters

In order to get our livescore data you need to build a corresponding URL with GET parameters, specifying what data to be retrieved. 

## General API Information

* The base URL is: **https://livescorefeedxml.com/api/**
* All pages return either a JSON object or XML.
* In order to use the API you need to have permanent redirect 301 enabled in the function you are using for getting the XMLs from our server.

If you are using PHP, this means that you should one of the following:

* Use file_get_contents():
 
```
file_get_contents($url);
```

* use curl with CURLOPT_FOLLOWLOCATION enabled:
```
$curl = curl_init();
curl_setopt($curl, CURLOPT_FOLLOWLOCATION, 1);
curl_exec($curl);
```

 
## Parameters

All parameters are sent via ```GET``` method to the base URL of the corresponding sport.
The URL of the sport is the base URL specified above + the sport name:

```https://livescorefeedxml.com/api/SPORT_NAME```

The SPORT_NAME has the following possible values:

```soccer```
```basketball```
```baseball```
```am_football```
```hockey```
```tennis```
```handball```



The table below describes the different type of parameters you can send to the API.

**Parameters:**

Name | Description | Mandatory | Possible values | Applicable sports
------------ | ------------ | ------------ | ------------ |  ------------ 
**key** | Your license key | yes | valid API key | all
**date** | Date restriction for the feed. If you specify this parameter you will get games only for the selected date. If you don’t specify this parameter, you will see all games for today. | no | date in YYYY-MM-DD format | all
**country_id** | Can be used in combination with the date parameter. If specified, filters the sport events by specific country. | no | any valid country id (You can request the list of countries over email) | all
**timezone_id** | Modifies the start hours of the games. If not defined, the feeds are displayed by default in the UTC/GMT +2 hours timezone. | no | any valid timezone id (You can find the list of timezone ids below) | all
**games** | If set to ```all``` displays all games for the specified period. If set to ```livescores``` or ```fixtures``` returns only livescore (games updated live) or only fixtures (games updated only with final result) | yes, unless **h2h** param is specified |  ```all``` , ```livescores```, ```fixtures```  | all
**standings** | If set to ```true```, standing tables are included in the feed. | no | ```true```, ```false``` | ```soccer```, ```basketball```, ```baseball```, ```am_football```, ```hockey```, ```handball```
**eventinfo** | If set to ```true```, event data - goals, cards, substitutions, etc... are included in the feed. | no | ```true```, ```false``` | ```soccer```,  ```hockey```
**lineups** | If set to ```true```, lineups are included in the feed. | no | ```true```, ```false``` | ```soccer```
**livestats** | If set to ```true```, livestats and live commentaries are included in the feed. | no | ```true```, ```false``` | ```soccer```
**h2h** |	Shows head 2 head statistics between the two teams for a specified game. | no | any valid game id | ```soccer```, ```basketball```, ```baseball```, ```am_football```, ```hockey```, ```handball```
**format** |	Specifies the format in which the feeds to be represented – JSON or XML. If not defined, by default the feeds are represented in a XML format. | no | ```xml```, ```json``` | all
**language** |	Returns the names of the teams and tournament in the specified language, if translation is availble. If there is not available translation for the particular entity, it is returned in english | no | ```en``` (English), ```fr``` (French), ```es``` (Spain), ```de``` (German), ```ru``` (Russian) | ```soccer```


## Examples


* Get all soccer games for today in XML format

```
https://livescorefeedxml.com/api/soccer?key=YOUR_KEY&games=all
```

* Get all games for today in JSON format

```
https://livescorefeedxml.com/api/soccer?key=YOUR_KEY&games=all&format=json
```

* Get all games for a specific date

```
https://livescorefeedxml.com/api/soccer?key=YOUR_KEY&games=all&date=YYYY-MM-DD
```

* Get all games for a specific date and include standings tables, eventinfo and livestats

```
https://livescorefeedxml.com/api/soccer?key=YOUR_KEY&games=all&date=YYYY-MM-DD&standings=true&eventinfo=true&livestats=true
```

* Get head to head stats for specific game

```
https://livescorefeedxml.com/api/soccer?key=YOUR_KEY&h2h=GAME_ID
```


* Get all tennis game for today

```
https://livescorefeedxml.com/api/tennis?key=YOUR_KEY
```

## Returned data structure



The retrieved data has the same structure hierarchy both in JSON and XML format.

Example of the main data structure for soccer:

```
{
    "maininfo": {
        "123456": {
            "tournament_id": 123, // main tournament id, e.g. Spain Tercera división
            "season": "2018\/2019", 
            "country_id": "205",
            "champ_id": "123456", // champ_id - id of the current season and
                                  //the particular subtournament, if any, e.g.: Spain Tercera división, 4th group, 2017/2018
            "champname": "Parva Liga",
            "country": "Bulgaria",
            "games": [{
                "id": "123061505",
                "home_score": "3",
                "away_score": "2",
                "status_type": "finished",
                "round": "11",
                "home_ordinary_score": "3", // scores within the regular time, excluding 90'-120' additional time 
                "away_ordinary_score": "2",
                "last_timestamp": "2017-12-30 00:13:23", // last update
                "status_desc_id": "15", //id of the status description
                "date": "2017-12-30",
                "hour": "10:30",
                "host": "Levski Sofia",
                "host_id": "200304575",
                "guest": "Botev Plovdiv FC",
                "guest_id": "33336776",
                "half_time__home_score": "0", 
                "half_time__away_score": "0",
                "min": "", // minute, displayed only if within the first or second half
                "eventinfo": {
                    ...
                },
                "status_desc": "Finished", // description of the status
                "hash": "892bd615d90969f8603748b105666dcbe00af5da" // hash of the last updated data
            }, ...
            ]
```

The ```hash``` value helps you track if the specific game have any updated fields since the last API call.
If the hash is the same as the one in your database, there is no need to update your database again.

### Time zones

The table below shows the possible values of the timezone_id parameter.

**Timezone ids:**

Timezone name | Id
------------ | ------------
UTC/GMT +12:45	| **40**
UTC/GMT +12	| **11**
UTC/GMT +11:30	| **39**
UTC/GMT +11| 	**10**
UTC/GMT +10:30	| **38**
UTC/GMT +10	| **9**
UTC/GMT +9:30| 	**37**
UTC/GMT +9| 	**8**
UTC/GMT +8:45| 	**36**
UTC/GMT +8| 	**7**
UTC/GMT +7| 	**6**
UTC/GMT +6:30| 	**35**
UTC/GMT +6| 	**5**
UTC/GMT +5:45| 	**34**
UTC/GMT +5:30| 	**33**
UTC/GMT +5| 	**4**
UTC/GMT +4:30| 	**32**
UTC/GMT +4| 	**3**
UTC/GMT +3:30| 	**31**
UTC/GMT +3	| **2**
UTC/GMT +2| 	**1**
UTC/GMT +1| 	**12**
UTC/GMT| 	**14**
UTC/GMT -1	| **26**
UTC/GMT -2| 	**15**
UTC/GMT -3| 	**16**
UTC/GMT -3:30	| **30**
UTC/GMT -4	| **17**
UTC/GMT -4:30	| **29**
UTC/GMT -5	| **18**
UTC/GMT -6	| **19**
UTC/GMT -7	| **20**
UTC/GMT -8	| **21**
UTC/GMT -9	| **22**
UTC/GMT -9:30| 	**28**
UTC/GMT -10	| **23**
UTC/GMT -11	| **24**
UTC/GMT -12	| **25**
