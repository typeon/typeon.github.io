---
layout: post
title:  "터미널에서 무비를 보고 싶다."
date:   2016-07-17 10:04:00 +0900
categories: util
---

리눅스의 장점을 꼽으라면 정말 말도 안되는 기능도 찾으면 다 구현되어 있다는 점입니다.
이번에는 터미널 상에서 무비를 플레이하는 방법을 살펴봅니다.

If you use Ubuntu or other Debian-based distro, the command to install mplayer will be:

```
sudo apt-get install mplayer
```

Now, to watch videos in the terminal, you just need to open the terminal and run this command:

```
mplayer -vo caca /path/to/the/video
```

If you just need to watch in black and white mode, then the command will be:

```
mplayer -vo aa /path/to/the/video
```

unfortunately, the video quality is not really good, you will only see the video being played in ASII mode.
However, the coolness value is undeniable. Here is the screenshot of a vid being played in the terminal

하나 더, 이번에 업무가 비디오, 오디오 핑거프린터 추출 및 인식과 관련된 내용이라 비디오 데이터를 수집해야 하는 경우가 생겼습니다.
그런 관계로 유투브 동영상을 command line에서 다운받는 유틸을 찾았습니다.

Run

```
# bash
sudo apt-get install youtube-dl
# python
sudo pip install youtube-dl
```

Then run

```
youtube-dl YouTube-video-link
```