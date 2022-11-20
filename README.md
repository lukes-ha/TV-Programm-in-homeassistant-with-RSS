# TV-Programm-in-homeassistant-with-RSS

**TV Programm in homeassistant**

_Germany_

The problem I want to solve:
I want a sensor, who is turned on/ true if one of my favorite shows comes on tv.

First Solution:

This solution is not the optimum, but with a rss feed, I get a few information.

I use the feed from Tv Spielform: https://www.tvspielfilm.de/services/widgets/rss-feeds/rss-feeds-im-ueberblick,3538128,ApplicationArticle.html
So I added this to my config:

`  - platform: feedparser
    name: TVProgramm
    feed_url: "https://m.tvspielfilm.de/tv-programm/rss/heute2015.xml"
    date_format: "%a, %d %b %Y %H:%M:%S %Z"
    scan_interval:
      hours: 1
    inclusions:
      - title
      - description`
      
And with this infos, a sensor turns on, if the show is in the tv program
config:

`template:
  - binary_sensor:
    - name: "Ninja Warrior"
      unique_id: "Ninja.Warrior.on.TV"
      state: "{{ 'Ninja Warrior' in  (state_attr ('sensor.tvprogramm','entries')) | map (attribute='title') | join ('|\n|') | replace ('|','') }}"
  - binary_sensor:
    - name: "Ninja Warrior 20:15"
      unique_id: "Ninjawarrior.20.15"
      state: "{{ '20:15  RTL  Ninja Warrior' in  (state_attr ('sensor.tvprogramm','entries')) | map (attribute='title') | join ('|\n|') | replace ('|','')`

This first one searches only for the words: "Ninja Warrior", because a program is often repeated at around 2 a.m. in the morning, to rule this out, I wrote the time before it, since it is directly in front of it in the string that the program outputs.



![Image](https://user-images.githubusercontent.com/107544776/202921260-b5807c28-6b11-4e81-8f4c-7952cf196d0f.png)



