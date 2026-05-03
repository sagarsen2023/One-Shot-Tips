## Instructions of implementing expo-audio for background audio playback.

In the app.json at `expo.plugins` paste the following

### For Android
```json
     "plugins": [
       [
        "expo-audio",
        {
          "enableBackgroundPlayback": true
        }
      ],
    ]
```
### For iOS
```json
     "infoPlist": {
        "ITSAppUsesNonExemptEncryption": false,
        "NSCameraUsageDescription": "This app requires access to the camera for QR Scanning facility.",
        "UIBackgroundModes": ["remote-notification", "audio"]
      }
```
Create a development build or run `npx expo run:android` or `npx expo run:ios`

Sample audio player that supports background audio playing after implementing the config plugin
```tsx
import React, { useEffect, useMemo, useRef } from "react";
import { ActivityIndicator, StyleSheet, View } from "react-native";
import {
  useAudioPlayer,
  useAudioPlayerStatus,
  setAudioModeAsync,
} from "expo-audio";
import { Pause, Play } from "lucide-react-native";
import { useThemeColors } from "@/src/constants/color-pallette";
import ThemeText from "./theme-text";
import { formatDuration } from "@/src/utils/format-date";
import { FONT_SIZE } from "@/src/constants/sizes";
import Button from "./button";
import SeekBar from "./seek-bar";

interface AudioPlayerProps {
  audioUrl: string;
  onPlay?: () => void;
  onEnd?: () => void;
  onPause?: () => void;
}

export default function AudioPlayer({
  audioUrl,
  onPlay,
  onPause,
  onEnd,
}: AudioPlayerProps) {
  const { text, primaryText, primary } = useThemeColors();

  const options = useMemo(
    () => ({ downloadFirst: true, updateInterval: 250 }),
    [],
  );
  const player = useAudioPlayer(audioUrl, options);
  const status = useAudioPlayerStatus(player);

  const finishedRef = useRef(false);

  useEffect(() => {
    // Configure audio session for background playback
    setAudioModeAsync({
      playsInSilentMode: true, // -> Required for ios.
      shouldPlayInBackground: true,
      interruptionMode: "doNotMix",
    });
  }, []);

  // Mark finished (and fire callback)
  useEffect(() => {
    if (status.didJustFinish) {
      finishedRef.current = true;
      onEnd?.();
    }
  }, [status.didJustFinish, onEnd]);

  const duration = status.duration ?? 0;
  const currentTime = status.currentTime ?? 0;

  const handlePress = async () => {
    if (!status.isLoaded) return;

    if (status.playing) {
      player.pause();
      player.setActiveForLockScreen(false);
      onPause?.();
      return;
    }

    // If last playback ended (or we're basically at the end), restart from 0.
    if (
      finishedRef.current ||
      (duration > 0 && currentTime >= duration - 0.25)
    ) {
      await player.seekTo(0); // required to replay from start [web:2]
      finishedRef.current = false;
    }

    player.play();
    // Adjust based on your implementation
    player.setActiveForLockScreen(
      true,
      {
        title: "Playing from my app",
        albumTitle: "An app by sagarsen2023",
        artist: "Sagar Sen",
        artworkUrl:
          "https://images.unsplash.com/photo-1618609377864-68609b857e90?q=80&w=3628&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D",
      },
      {
        showSeekBackward: true,
        showSeekForward: true,
      },
    );
    onPlay?.();
  };

  return (
    <View style={styles.container}>
      <Button onPress={handlePress} style={styles.playPauseButton} size="sm">
        {status.isLoaded ? (
          status.playing ? (
            <Pause color={primaryText} size={20} />
          ) : (
            <Play color={primaryText} size={18} />
          )
        ) : (
          <ActivityIndicator size="small" color={primaryText} />
        )}
      </Button>

      <View style={styles.seekContainer}>
        <ThemeText style={styles.durationText}>
          {formatDuration(currentTime)}
        </ThemeText>

        <SeekBar
          value={Math.min(currentTime, duration || 1)}
          max={duration || 1}
          trackColor={text + "20"}
          fillColor={primary}
          thumbColor={primary}
          onSeekStart={() => {}}
          onSeek={(v) => {
            player.seekTo(v);
            finishedRef.current = false;
          }}
          onSeekComplete={(v) => {
            player.seekTo(v);
            finishedRef.current = false;
          }}
        />

        <ThemeText style={styles.durationText}>
          {formatDuration(duration)}
        </ThemeText>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    width: "100%",
    justifyContent: "center",
    alignItems: "center",
    flexDirection: "row",
    gap: 10,
  },
  playPauseButton: {
    justifyContent: "center",
    alignItems: "center",
    borderRadius: 999,
    width: 35,
    height: 35,
  },
  seekContainer: {
    flex: 1,
    flexDirection: "row",
    alignItems: "center",
    gap: 10,
  },
  slider: {
    flex: 1,
    height: 20,
  },
  durationText: {
    fontSize: FONT_SIZE.sm,
    opacity: 0.5,
  },
});

```
