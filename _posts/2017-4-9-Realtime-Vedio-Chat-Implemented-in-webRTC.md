---
layout: post
title: Realtime vedio chat implemented in webRTC
---

## 1. Overview
WebRTC enables peer to peer comunication. But server is still needed for signaling (exchange metadata between peers). And webRTC does not define the signaling protocal itself, you can implement it anyway you like, as long as certain info that webRTC needed could be exchange successfully.

![image](http://note.youdao.com/favicon.ico)

In this image, you could see there are two parts: signaling and peer to peer communication via webRTC. I could disscuss the two parts with the help of an open source implemetation: [nextRTC](https://nextrtc.org/), it has provided a wrapper of webRTC offical api and an implementation of signaling server in Java. In this article, we would only consider the vedio chat in a LAN, so NAT and firewall are not discussed.

## 2. webRTC
I think webRTC maybe the easier part, as it has api offically. In below picture you could see where it is in the whole architecture.

![image](http://note.youdao.com/favicon.ico)

Without considering the signaling mechanisim, using the offical api, RTCPeerConnection, the general process of A calling B is:
1. A create a RTCPeerConnection, createOffer, setLocalDescription, using signaling mechanism send SDP(session description metadata, which is the necessary for peer to peer connection) to B, more specifically see below code;

        var pc = new RTCPeerConnection(peerConfig);
        pc.createOffer({offerToReceiveAudio: 1, offerToReceiveVideo: 1})
        .then(function(desc) {
            pc.setLocalDescription(desc)
                .then(function() {
                    // This is the code nextRTC use to do the signaling.
                    nextRTC.request('offerResponse', signal.from, desc.sdp);
                }, error);
        });
2. B setRemoteDescription with A's offer, create answer, setLocalDescription, using signaling mechanism send answer to A;

        // This is the nextRTC way to manage the RTCPeerConnection objects.
        var pc = nextRTC.preparePeerConnection(nextRTC, signal.from);
		pc['pc'].setRemoteDescription(new RTCSessionDescription({
			type : 'offer',
			sdp : signal.content
		})).then(function() {
    		pc['rem'] = true;
        	pc['pc'].createAnswer().then(function(desc) {
    		    pc['pc'].setLocalDescription(desc).then(function() {
        		    nextRTC.request('answerResponse', signal.from, desc.sdp);
        	    });
            });
        });
3. A setRemoteDescription with B's answer;

        var pc = nextRTC.preparePeerConnection(nextRTC, signal.from);
		pc['pc'].setRemoteDescription(new RTCSessionDescription({
			type : 'answer',
			sdp : signal.content
		})).then(function(){
			pc['rem'] = true;
		});
		
Above's code is just for giving a sign, you could find nextRTC.js from [here](https://nextrtc.org/). You may have questions now, like, how did peer find each other? How did metadata send to a specified peer? This is most the job that signaling server would do.

## 3. signaling server
Signaling server could be implemented in many exsiting protocal. NextRTC choose websocket. The implementation of signaling server could also be split into two parts in my thinking: 1. create or join a chat room; 2. when participants are ready, exchange info that webRTC needed, and drive the three steps for webRTC introduced above.
### A. create or join a conversation
This is how peers find each other. In the webRTC part, I have introduced the three steps to build a peer to peer connection, but I have missed the part how A send metadata to B. Now I would expain that. First A need to know there is a B, vice versa. To achieve this, there are 3 steps:
1. A send a message to server via websocket, like "create", with or without a conversation Id (convId). If there is none, a UUID would be created as a convId. This would be stored in server with data structure Map<String, Conversation>, which the key is the convId, and value is the conversation.
2. A send the convId to B, no matter it is generated or not. B send message to server, "join", with the convId.

Now the conversation in server would have two members.

### B. exchange and drive the webRTC to happen
Finally, A and B would like to exchange vedio metadata.
1. signaling server -> "offerRequest" -> A;
2. A:execute webRTC step1 -> "offerResponse" -> signaling server;
3. signaling server -> "answerRequest" -> B;
4. B:execute webRTC step2 -> "answerResponse" -> signaling server;
5. signaling server -> "finalize" -> A;
6. A:execute webRTC step3.

Now all the process, including webRTC and signaling are finished. During the exchange process, there is a data structure, Table<Member, Member, ConnectionContext>, which store the connectionContext between Member A and B (A and B, B and A would have the same context instance). The context would store the connectionState, and each time receiving message("offerRequest","answerRequest") from peer, it is actually the context decide how to process according to the connectionState.

By the way, this is just one way to implement the signaling server.
