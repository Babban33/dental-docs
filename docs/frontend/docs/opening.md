---
id: opening
title: Opening.jsx
sidebar_position: 4
---

# `Opening.jsx` Page

The `MouthOpening` component allows users to select a camera, stream video from it, and capture a photo of their mouth opening. The photo is sent to a backend server for analysis, and the results are displayed. The component also handles camera selection, toggling streaming, and displaying the results with options to capture another image.

## State Variables

```javascript
const [cameras, setCameras] = useState([]);
const [selectedCamera, setSelectedCamera] = useState("");
const [streaming, setStreaming] = useState(false);
const [mediaStream, setMediaStream] = useState(null);
const [capturedPhoto, setCapturedPhoto] = useState(null);
const [photoClicked, isPhotoClicked] = useState(true);
const [selectedImage, setSelectedImage] = useState(null);
const [generatedImage, setGeneratedImage] = useState(null);
const [length, setLength] = useState(null);
const [showPopup, setShowPopup] = useState(false);
const [language, setLanguage] = useState('en');
```
- **`cameras`**: Holds an array of available camera devices. It is populated when the component loads using `navigator.mediaDevices.enumerateDevices()`.
- **`selectedCamera`**: Stores the ID of the currently selected camera. Initially set to an empty string.
- **`streaming`**: A boolean that tracks whether the camera stream is active. It is toggled by the user.
- **`mediaStream`**: Holds the media stream object returned by `navigator.mediaDevices.getUserMedia()` for the selected camera.
- **`capturedPhoto`**: Stores the URL of the captured photo, which is displayed to the user.
- **`photoClicked`**: A boolean flag used to check if the photo has been clicked. Initially set to `true`.
- **`selectedImage`**: Stores the captured photo as a `File` object to send to the server.
- **`generatedImage`**: Stores the generated image (processed by the server) in base64 format to be displayed.
- **`length`**: Represents the detected mouth opening value returned from the server.
- **`showPopup`**: A boolean that controls whether the video stream popup is shown or not.
- **`language`**: Stores the current language setting (default is `'en'`). This is used to fetch localized content from `data.json`.

## useEffect Hook
```javascript
useEffect(() => {
    const getAvailableCameras = async () => {
        try {
            const devices = await navigator.mediaDevices.enumerateDevices();
            const videoDevices = devices.filter(device => device.kind === 'videoinput');
            setCameras(videoDevices);
            if (videoDevices.length > 0) {
                setSelectedCamera(videoDevices[0].deviceId);
            }
        } catch (error) {
            console.error('Error accessing media devices:', error);
        }
    };

    getAvailableCameras();

    const getLang = () =>{
        const lang = localStorage.getItem('lang');
        setLanguage(lang);
    }
    getLang();
}, []);
```
The `useEffect` hook runs when the component is mounted and does the following:
- **Fetching available cameras**: Calls `navigator.mediaDevices.enumerateDevices()` to fetch available camera devices and filters for video input devices. The first camera is selected by default if available.
- **Language setting**: Retrieves the stored language preference from `localStorage` using `localStorage.getItem('lang')` and updates the `language` state.


## `handleCameraChange` Event Handler
```javascript
const handleCameraChange = (event) => {
    setSelectedCamera(event.target.value);
};
```
Updates the selectedCamera state when the user selects a new camera from the dropdown.

## `toggleStreaming`
```javascript
const toggleStreaming = () => {
    if (streaming) {
        mediaStream.getTracks().forEach(track => track.stop());
        setMediaStream(null);
    } else {
        startStreaming();
    }
    setStreaming(!streaming);
};
```
Toggles the camera streaming on and off. If streaming is active, it stops the stream; otherwise, it starts the stream by calling `startStreaming()`.

## `startStreaming`
```javascript
const startStreaming = async () => {
    try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: { deviceId: selectedCamera } });
        setMediaStream(stream);
    } catch (error) {
        console.error('Error starting streaming:', error);
    }
};
```
Initiates streaming from the selected camera using `navigator.mediaDevices.getUserMedia()`.

## `captureAndCheckOpening`
```javascript
const captureAndCheckOpening = async () => {
    try {
        const track = mediaStream.getVideoTracks()[0];
        const imageCapture = new ImageCapture(track);
        const photoBlob = await imageCapture.takePhoto();
        const photoUrl = URL.createObjectURL(photoBlob);
        setCapturedPhoto(photoUrl);
        isPhotoClicked(false);

        const pngFile = new File([photoBlob], "captured_photo.png", { type: "image/png" });
        setSelectedImage(pngFile);

        const formData = new FormData();
        formData.append('file', pngFile);

        const response = await fetch("http://127.0.0.1:8000/opening", {
            method: 'POST',
            body: formData
        });

        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const data = await response.json();
        setGeneratedImage(data.generatedImage);
        setLength(data.opening);
    } catch (error) {
        console.error('Error capturing photo or from Server:', error);
    } finally {
        if (streaming) {
            mediaStream.getTracks().forEach(track => track.stop());
            setMediaStream(null);
        }
        setShowPopup(false);
    }
};
```
- The function first captures a photo from the webcam using the `ImageCapture` API.
- It then prepares the photo for uploading by converting it into a `File` object.
- The photo is sent to a server using `fetch` in a `POST` request.
- The server's response is processed, and relevant data (like a generated image and opening dimension) is set in the component's state.

## `captureAgain`
```javascript
const captureAgain = async () => {
    setCapturedPhoto(null);
    setShowPopup(true);
    setGeneratedImage(null);
    setLength(null);
    try {
        const stream = await navigator.mediaDevices.getUserMedia({ video: { deviceId: selectedCamera } });
        setMediaStream(stream);
    } catch (error) {
        console.error('Error starting streaming:', error);
    }
    isPhotoClicked(true);
};
```
The function resets the application state, initiates a new video stream from the selected camera, and prepares the system for the next photo capture attempt.