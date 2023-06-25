<template>
  <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    />
  </q-page>
</template>

<script setup>
import axios from "axios";
import { onMounted } from "vue";
const subtitles = [];

/*
- Leer subtitulos
- Coincidir con el video

*/

function parseSrtFile(srtText) {
  const subtitleRegex =
    /(\d+)\n(\d{2}:\d{2}:\d{2},\d{3}) --> (\d{2}:\d{2}:\d{2},\d{3})\n([\s\S]*?(?=\n{2}|$))/g;
  const subtitles = [];

  let match;
  while ((match = subtitleRegex.exec(srtText)) !== null) {
    const subtitle = {
      id: parseInt(match[1]),
      start: match[2],
      end: match[3],
      text: match[4].trim(),
    };
    subtitles.push(subtitle);
  }

  return subtitles;
}

const loadSubtitles = async () => {
  try {
    const response = await axios.get("subtitles.srt");
    console.log(response);
    const srtText = response.data;
    subtitles = parseSrtFile(srtText);
  } catch (error) {
    console.error("Failed to load subtitles", error);
  }
};
loadSubtitles();
</script>
