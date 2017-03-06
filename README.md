# http-widget
Offline control mechanism for OpenAPS

Android "HTTP widget" allows you to show text inside the widget, perfect for OpenAPS monitoring without using Nightscout (in case no internet access is available).

![http-widget](https://cloud.githubusercontent.com/assets/12679825/23367009/0a582f3e-fd09-11e6-93fc-02fb14bc8457.JPG)



- install "HTTP widget" on your Android mobil 
https://play.google.com/store/apps/details?id=net.rosoftlab.httpwidget1&hl=en

- install jq

`sudo apt-get install jq`

- add the following cron line to your crontab (where `myopenpas` is your openaps init directory)

`@reboot cd /root/myopenaps/enact && python -m SimpleHTTPServer 1337`

- add the following openaps alias to your `openaps.ini`

`http-widget = ! bash -c "( jq .timestamp ~/myopenaps/enact/enacted.json | awk '{print substr($0,13,5)}' && jq -r .reason ~/myopenaps/enact/enacted.json && echo -n 'TBR: ' && jq .rate ~/myopenaps/enact/enacted.json && echo -n 'IOB: ' && jq .IOB ~/myopenaps/enact/enacted.json && echo -n 'Battery: ' && (~/src/EdisonVoltage/voltage short) | awk '{print $2,$1}' ) > ~/myopenaps/enact/index.html"`
- modify the `openaps pump-loop` cron line to (`openaps http-widget` needs to be run each minute)

`* * * * * cd /root/myopenaps && ( ps aux | grep -v grep | grep -q 'openaps pump-loop' || openaps pump-loop ) 2>&1 | tee -a /var/log/openaps/pump-loop.log && openaps http-widget > /dev/null 2>&1`
`
- On your Android phone add a HTTP widget as you know how to add widgets normally and set the the http with your rig IP address using port 1337

![add-widget](https://cloud.githubusercontent.com/assets/12679825/23367485/b5d3f93c-fd0a-11e6-9ee1-f6d6c04b1c25.JPG)


`


