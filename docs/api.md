# Low Level SDK

The low level SDK provides Dyte's core functionality, while letting a developer build a custom UI over it.

# Table of Contents

- [API](#api)
- [Events](#events)

# API
## DyteClient

The `DyteClient` class exposes the Low Level APIs. The `init` method can be used to create an instance of `DyteClient`. The object created exposes methods to carry out various functionalities on Dyte, as described in the [`DyteClient.init()`](#dyteclient.init()) section.

## DyteClient.init()

### Parameters:

The `init()` method accepts an object consisting of `roomName`, `authToken`, `apiBase` (optional), and `defaults` (optional).

The following interface describes the parameters for the `init()` method.

```ts
interface DyteClientInitParams {
    roomName: string,
    authToken: string,
    apiBase?: string,
    defaults?: { video?: boolean, audio?: boolean },
}
```

| **Parameter** | **Description**                                               | **Necessity** | **Default**                    |
|---------------|---------------------------------------------------------------|---------------|--------------------------------|
| `roomName`    | The meeting `roomName` received using the backend API call.   | required      | undefined                      |
| `authToken`   | Participant `authToken` received in the backend API call.     | required      | undefined                      |
| `apiBase`     | The base URL for the backend API environment.                 | optional      | `https://api.cluster.dyte.in`  |
| `defaults`    | Define which media streams will be fetched on initialization. | optional      | `{ video: true, audio: true }` |

```ts
const meeting = await DyteClient.init({
    roomName: 'iqmhcy-gujwkw',
    authToken: 'eyJhbGciOiJIUz...',
});
```

> Note: The `meeting` object created above exposes methods for interacting with the Dyte meeting. Any reference to **meeting** in the rest of the documentation refers to this object.

## meeting.self

The `meeting.self` object is used to refer to the current user's media streams, and other details of the user.

The `meeting.self` public properties can be described using the following interface:

```ts
interface DyteSelf {
    id: string,
    name: string,
    clientSpecificId: any,
    audioTrack: MediaStreamTrack,
    videoTrack: MediaStreamTrack,
    screenShareTracks: {
        audio: MediaStreamTrack,
        video: MediaStreamTrack,
    },
    audioEnabled: boolean,
    videoEnabled: boolean,
    screenShareEnabled: boolean,
    permissions: {
        audio?: 'ACCEPTED' | 'DENIED';
        video?: 'ACCEPTED' | 'DENIED';
    },
    roomJoined: boolean,
}
```

### Properties

| **Property** | **Data Type**                                                      | **Description**                                                                           |
|-----------------------|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| id                    | `string`                                                             | Peer ID of the current user.                                                              |
| name                  | `string`                                                             | Name of the current user.                                                                 |
| clientSpecificId      | `any`                                                                | Client specific ID for the participant that was passed to the participant API.            |
| audioTrack            | `MediaStreamTrack`                                                   | The current user's audio stream track that can be passed to the audio element in the DOM. |
| videoTrack            | `MediaStreamTrack`                                                   | The current user's video stream track that can be passed to the video element in the DOM. |
| screenshareTracks     | `{ audio: MediaStreamTrack, video: MediaStreamTrack }`               | The audio and video stream tracks of the screen that is being shared by the current user. |
| audioEnabled          | `boolean`                                                            | Represents if the current user is producing audio.                                        |
| videoEnabled          | `boolean`                                                            | Represents if the current user is producing video.                                        |
| screenshareEnabled    | `boolean`                                                            | Represents if the current user is sharing their screen.                                   |
| permissions           | `{ audio?: 'ACCEPTED' \| 'DENIED', video?: 'ACCEPTED' \| 'DENIED' }` | Represents which browser permissions (audio and video) were accepted by the user.         |
| roomJoined            | `boolean`                                                            | Represents whether the user has successfully joined the room.                             |

### Methods

| **Method**           | **Parameters**                                                                | **Return Type**     | **Description**                                                                                                                                                                                         |
|----------------------|-------------------------------------------------------------------------------|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| setupTracks          | `defaults: {video?: boolean, audio?: boolean}`                                | `Promise<void>`     | The setupTracks method can be used to fetch the audio and video streams from user's device. This is called by default upon room joining unless explicitly set to false in `await DyteClient.init(...)`. |
| enableAudio          | `undefined`                                                                   | `Promise<void>`     | Enables the audio of the current user if they are present in the room.                                                                                                                                  |
| enableVideo          | `undefined`                                                                   | `Promise<void>`     | Enables the video of the current user if they are present in the room.                                                                                                                                  |
| enableScreenShare    | `undefined`                                                                   | `Promise<void>`     | Starts sharing the screen of the current user to the room.                                                                                                                                              |
| disableAudio         | `undefined`                                                                   | `Promise<void>`     | Disables the audio of the current user if they are present in the room.                                                                                                                                 |
| disableVideo         | `undefined`                                                                   | `Promise<void>`     | Disables the video of the current user if they are present in the room.                                                                                                                                 |
| disableScreenShare   | `undefined`                                                                   | `Promise<void>`     | Stops sharing the screen of the current user to the room.                                                                                                                                               |
| sendChatTextMessage  | `message: string`                                                             | `void`              | Sends a text message to the room.                                                                                                                                                                       |
| sendChatImageMessage | `link: string`                                                                | `void`              | Sends an image to the room.                                                                                                                                                                             |
| sendChatFileMessage  | `link: string, name: string, size: number`                                    | `void`              | Sends a file to the room.                                                                                                                                                                               |
| createPoll           | `question: string, options: string[], anonymous: boolean, hideVotes: boolean` | `void`              | Create a poll in the room.                                                                                                                                                                              |
| voteOnPoll           | `pollId: string, index: number`                                               | `void`              | Votes on a poll in the room.                                                                                                                                                                            |
| getAudioDevices      | `undefined`                                                                   | `MediaDeviceInfo[]` | Returns the audio devices available to the current user.                                                                                                                                                |
| getVideoDevices      | `undefined`                                                                   | `MediaDeviceInfo[]` | Returns the video devices available to the current user.                                                                                                                                                |
| getSpeakerDevices    | `undefined`                                                                   | `MediaDeviceInfo[]` | Returns the speaker devices available to the current user.                                                                                                                                              |
| getDeviceById        | `deviceId: string, kind: 'audio' \| 'video' \| 'speaker'`                     | `MediaDeviceInfo`   | Returns the device available to the current user, filtered by the device ID and kind.                                                                                                                   |
| getAllDevices        | `undefined`                                                                   | `MediaDeviceInfo[]` | Returns all the devices available to the current user.                                                                                                                                                  |
| setDevice            | `device: MediaDeviceInfo`                                                     | `Promise<void>`     | Sets the `device` for user media stream input or output, based on the kind of the devices.                                                                                                              |

## meeting.joinRoom()

The `meeting.joinRoom()` method can be called in order to join a room.

```ts
await meeting.joinRoom();
```

| **Method** | **Parameters** | **Return Type** | **Description**                    |
|------------|----------------|-----------------|------------------------------------|
| joinRoom   | `undefined`    | `Promise<void>` | Adds the current user to the room. |

## meeting.room

The `meeting.room` object describes the meeting room.

This can be described with the following interface.

```ts
interface DyteRoom {
    roomName: string;

    roomType: RoomViewType;

    chatMessages: ChatMessage[];

    polls: Poll[];
}
```

The types used in the above interface are defined below.

```ts
enum RoomViewType {
    webinar = 'webinar',
    groupCall = 'group_call',
    audioRoom = 'audio_room',
}

interface ChatMessageBase {
    userId: string;
    displayName: string;
    read?: boolean;
    pluginId?: string; // Present if the message is from a plugin.
}

interface TextMessage extends ChatMessageBase {
    type: MessageTypes.text;
    message: string;
}

interface ImageMessage extends ChatMessageBase {
    type: MessageTypes.image;
    link: string;
}

interface FileMessage extends ChatMessageBase {
    type: MessageTypes.file;
    name: string;
    size: number;
    link: string;
}

interface PollMessage extends ChatMessageBase {
    type: MessageTypes.poll;
    pollId: string;
}

type ChatMessage = TextMessage | ImageMessage | FileMessage | PollMessage;

interface PollOption {
    text: string;
    votes: { id: string, name: string }[];
    count: number;
}

interface Poll {
    id: string;
    question: string;
    options: PollOption[];
    anonymous: boolean;
    hideVotes: boolean;
    createdBy: string;
}
```
### Properties

| **Property** | **Data Type**   | **Description**                                             |
|--------------|-----------------|-------------------------------------------------------------|
| roomName     | `string`        | Name of the current room that the user is a part of.        |
| roomType     | `RoomViewType`  | Type of the room: `webinar`, `group_call`, or `audio_room`. |
| chatMessages | `ChatMessage[]` | A list of messages present in the room chat.                |
| polls        | `Poll[]`        | A list of polls present in the room.                        |

## meeting.participants

The `meeting.participants` object exposes the list of participants and methods associated with them.

The `meeting.participants` object can be described using the following interface.

```ts
interface DyteParticipants {
    waitlisted: DyteParticipantMap,
    joined: DyteParticipantMap,
    active: DyteParticipantMap,
    pinned: DyteParticipantMap,
}
```

### Properties

| **Property** | **Return Type**      | **Description**                                                 |
|--------------|----------------------|-----------------------------------------------------------------|
| waitlisted   | `DyteParticipantMap` | Map of waitlisted participants.                                 |
| joined       | `DyteParticipantMap` | Map of all participants.                                        |
| active       | `DyteParticipantMap` | Map of active participants who should be displayed in the grid. |
| pinned       | `DyteParticipantMap` | Map of pinned participants.                                     |

Each of `meeting.waitlisted`, `meeting.joined`, `meeting.active`, and `meeting.pinned` is of type `DyteParticipantMap`. This consists of key-value pairs where the keys are `peerId`s (of type `string`), and the values are `DyteParticipant` objects, an interface for which is defined below.

```ts
interface DyteParticipant {
    id: string;
    userId: string;
    name: string;
    picture: string;
    isHost: boolean;
    clientSpecificId?: string;
    flags: { [key: string]: string | boolean };
    device: DeviceConfig;
    videoTrack: MediaStreamTrack;
    audioTrack: MediaStreamTrack;
    videoEnabled: boolean;
    audioEnabled: boolean;
}
```

Each `DyteParticipantMap` object has the following methods.

| **Method** | **Parameters** | **Return Type**     | **Description**                           |
|------------|----------------|---------------------|-------------------------------------------|
| toArray    | `undefined`    | `DyteParticipant[]` | Returns the participant map as an array. |

# Events

The `meeting.self`, `meeting.room`, and `meeting.participants` objects are also event emitters, on which you can listen for events using `.on()`.

The emitter is of type `DyteEventEmitter`. All events emitted are of type:

```ts
event: '*' | 'roomJoined' | 'videoUpdate' | 'audioUpdate' | 'screenShareUpdate' | 'chatUpdate' | 'pollsUpdate' | 'participantJoined' | 'participantLeft'
```

## meeting.self

The `meeting.self` object emits the following events:

| **Event**         | **Callback Type**                                                                                                           | **Description**                         |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| audioUpdate       | `(params: { audioEnabled: boolean, audioTrack: MediaStreamTrack}) => void`                                                  | Audio was muted or unmuted.             |
| videoUpdate       | `(params: { videoEnabled: boolean, videoTrack: MediaStreamTrack}) => void`                                                  | The video was turned on or off.         |
| screenShareUpdate | `(params: { screenShareEnabled: boolean, screenShareTracks: { audio: MediaStreamTrack, video: MediaStreamTrack }}) => void` | Screen sharing was enabled or disabled. |
| roomJoined        | `undefined`                                                                                                                 | The room was joined.                    |

### Example

```ts
meeting.self.on('audioUpdate', ({ audioEnabled, audioTrack }) => {
    console.log(audioEnabled);
    console.log(audioTrack);
});
```

## meeting.room

The `meeting.room` object emits the following events.

| **Event**   | **Callback Type**                                                                             | **Description**              |
|-------------|-----------------------------------------------------------------------------------------------|------------------------------|
| chatUpdate  | `(params: { newMessage: boolean, message: ChatMessage[], messages: ChatMessages[] }) => void` | A chat update was received.  |
| pollsUpdate | `(params: { newPoll: boolean, polls: Poll[], updatedPoll: Poll }) => void`                    | A polls update was received. |

### Example

```ts
meeting.room.on('chatUpdate', ({ newMessage, message, messages }) => {
    console.log(newMessage);
    console.log(message);
    console.log(messages);
});
```

## meeting.participants

The `meeting.participants` has 4 properties, `waitlisted`, `joined`, `active`, `pinned`, each of which are of type `DyteParticipantMap`. These maps contains key-value pairs with keys of type `string` (`peerId`s) and values of type `DyteParticipant`. Now, each `DyteParticipant` object is an event emitter, and emits such as `audioUpdate`, `videoUpdate`, etc. The parent `DyteParticipantMap` also re-emits the same event after adding the `participant` object to the event payload.

### Example

```ts
meeting.participants.joined.toArray()[0].on('videoUpdate', ({ videoEnabled, videoTrack }) => {
    console.log(videoEnabled, videoTrack);
});

// The same event can also be listened for using the following manner.

meeting.participants.joined.on('videoUpdate', (participant, { videoEnabled, videoTrack }) => {
    console.log(participant, videoEnabled, videoTrack);
});
```

### Events emitted by a `DyteParticipant` object.

| **Event**         | **Callback Type**                                                                                                           | **Description**                         |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
| audioUpdate       | `(params: { audioEnabled: boolean, audioTrack: MediaStreamTrack}) => void`                                                  | Audio was muted or unmuted.             |
| videoUpdate       | `(params: { videoEnabled: boolean, videoTrack: MediaStreamTrack}) => void`                                                  | The video was turned on or off.         |

### Events emitted by a `DyteParticipantMap` object.

A `DyteParticipantMap` object re-emits the same events as `DyteParticipant`, as shown in the example above. Additionally, the following events are emitted.

| **Event**         | **Callback Type**                    | **Description**                         |
|-------------------|--------------------------------------|-----------------------------------------|
| participantJoined | `(participant: Participant) => void` | A new participant was added to the map. |
| participantLeft   | `(participant: Participant) => void` | A participant left the map.             |
