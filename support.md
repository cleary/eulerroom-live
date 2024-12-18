# Streaming Event Admin Support

**Purpose:** document support needs for TOPLAP streaming events so that event admins understand what is needed.

**Systems detail:** See the [OPS / Maintainance](OPS.md) guide for technical details, host info, operational service commands, etc. 

## Background

By design, streaming events should need only minimal admin support during the event. The Muxy system manages time slots and access to the rtmp services. When the host server is adquately provisioned, streaming is usually stable and reliable. When users read and follow the setup and streaming instructions, everything runs smoothly. However, users often have questions, problems do come up, and like any system, components can fail. 

Providing admin support depends on your level of knowledge, access, systems experience, and availability during an event. All help is appreciated even if you don't have the more advanced systems and streaming services expertise. 

## Support areas

1. Users (Basic)
    - questions
    - streaming problems
2. Streaming service failures (Intermediate)
3. Stream performance issues (Advanced)

## Responsibility during event

Summary (see sections below for details):
- Respond to users posting in the designated Discord channel.
- Monitor the streaming chat activity, post info if there is a problem.
- For admins with shell access to the host:
    - Monitor CPU resource utilization (`top` command)
    - restart services if there is a failure
    - monitor logs if there are problems
- (Advanced) monitor stream performance, consider changes if performance is consistently problematic.

### User Support

**Main Action:** Monitor the Discord channel:  
`TOPLAP live coding > stream-help`  
Respond where feasible. The key thing is just to be available. 

Sometimes it isn't possible to do anything. Problems can surface right before a stream will start. If it isn't a simple issue, it may not be possible to resolve it before the stream is over. 

Common problems / questions

- I can't connect
- I'm next up - can I start streaming now, or do I need to wait for the current stream to end?
- There is no audio in my stream.
- My connection to the test server isn't working.

Possible responses

- The test server only allows one user to stream at a time. If you can't connect, check the test streaming site to see if its busy. 
- For OBS problems, try restarting OBS. Check your Preferences against the streaming documentation. 
- For connection problems - make sure you have the correct streaming key and it is entered properly in the OBS Preferences. (Admins can verify the streaming key in [Muxy Admin](https://muxy.eulerroom.com/admin/login/?next=/admin/).)

Very occasionally, Muxy creates duplicate slots for the same user at the same time, with 2 different keys. This can be a problem. One of the duplicate streams should be removed. 

### Streaming Service Failures

This is **rare** but it has happened. One scenario is that the Owncast service hangs. Error messages may show up in the streaming chat. When this happens, there will be no streaming until the services are restored. 

**Response:**
- use the docker commands found in the *Restart a service* section of the [OPS / Maintainance](OPS.md#restart-a-service) guide.
    - the `main-owncast` service is the best place to start:  
    `docker compose up -d --force-recreate --build main-owncast`
    - you can also restart all services:  
    `docker compose up -d --force-recreate --build`

### Stream Performance (Advanced)

*Note:* Most admins aren't expected to engage at this level. It requires access into the Owncast admin UI. 

The most common problem is that the audio of a stream is choppy. This can happen when stream processing can't keep up with the amount of incoming stream data. Typically this is due to a bad stream, using a high screen resolution, high bitrate, high fps rate, or combination. (Some users don't follow the streaming guides, or don't allow time to figure it all out.)

Other problems:
- user has an inadequate internet connection - either low bandwidth  or an unreliable connection 
- user has a video intensive stream with motion heavy graphics (sometimes this is ok)

Challenges: 

- Nothing can be done during a bad stream. 
- Nothing can be done with user side problems, except to inform the user for future reference. 
- The Owncast settings have been adjusted as much as possible to accommodate bad streams - so there is not much more to be done.

**Adjusting Owncast stream output**
We provision 2 output streams - with a low bandwidth option for those watching/participating via lower bandwidth. The Owncast manual recommends operating only 1 stream output to conserve resources and then scaling up. So if stream performance is consistently choppy, one option is to delete the `Low bandwith` ouput stream. However, there is no consensus on using this option, and no clear indication that it will even help the choppy audio problem. 

**Worst case:**   
If audio problems persist across multiple streams, the quality of the event itself can become compromised. No one wants to experience streaming live coding where the audio is consistently choppy. The best resort is to resize the server and hope that throwing more CPU resources will help. See: [Resize](OPS.md#linode-host-operations) in the OPS / Maintenance guide. 

