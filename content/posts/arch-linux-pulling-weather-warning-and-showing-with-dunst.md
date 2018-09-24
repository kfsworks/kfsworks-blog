+++
title = "Arch Linux Pulling Weather Warning and Showing With Dunst"
date = 2018-09-24T05:32:04Z
tags = ["archlinux", "dunst", "golang"]
categories = []
+++

### Background
Recently the typhoon Mangkhut was hitting Hong Kong, and many of the trees are collapse due to this category 6 storm in one night. During that night while using my laptop, I was wondering whether there is anyway to popup some notification to the i3, and having some latest alert from Hong Kong Observatory. But I couldn't find one. So I quickly make up a simple daemon which would pull out the RSS from Hong Kong Observatory and send to dunst.

Below is a sample screen shot when there is an alert.
![Sample screen shot](https://blog.kafaiworks.com/images/weather-warning-sample.png)

### What language to use?
The source was written in golang, here is the [repository](https://github.com/kfsworks/weather-warning). Reason to use golang is this is a good language, as simple as that.

The program is very simple, and come with some simple tests. Please look at below diagram.
![Sample screen shot](https://blog.kafaiworks.com/images/weather-warning-diagram.png)

There is a struct which holding the weather warning information.
```
type WeatherWarning struct {
    Title       string
    Description string
    PubDate     time.Time
}
```

And there are 2 workers, one is fetching the data through the net, and the other one is publishing the data to the notification daemon (dunst). And these 2 worker just communicating by the WeatherWarning struct. In theory, the fetcher can be extended to multiple fetchers and they all just putting the result struct into the channel. This one cool thing that golang brought to us, concurrency and channel.
```
    var c chan warning.WeatherWarning = make(chan warning.WeatherWarning, 5)
    go sendNotification(c)
    go fetcher.Fetch(c)

```

### Final
If you feel like you want to do similar thing, feel free to use the code, enjoy!


