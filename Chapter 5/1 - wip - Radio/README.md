# Radio

```
Silence dominates the crumbling city. Our hearts race as we spot a radio tower. "This could be our salvation," we whisper. "Is it a distress signal or something sinister?" We approach cautiously. The generator looks intact. "We must get it running." With a deep breath, we pull the starter cord. The engine roars to life. "Here we go," we mutter, adjusting the radio.
Maybe we'll be able to find someone who owns a vehicle and can help us reach the shipyard.
```

Since the port we are presented in the challege is `1883` we know that is a message queque:
```
mosquitto_sub.exe -v -h <ip> -p 1883 -t #
```

Incoming data:
```
AAAAAAAAAAAAAAAAAAAAAAAAA
frequency/1337/read IyEvd...
frequency/3137/read UklGR...
```