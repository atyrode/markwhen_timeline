<script setup lang="ts">
import { useTimelineStore } from "../timelineStore";
import { computed } from "vue";
import TimelineScale from "../Settings/TimelineScale.vue";
import Minimap from "./Minimap.vue";
import AutoCenter from "./AutoCenter.vue";
import ToggleMiniMap from "./ToggleMiniMap.vue";
import CollapseAll from "./CollapseAll.vue";
import ExpandAll from "./ExpandAll.vue";
import ToggleMode from "./ToggleMode.vue";
import ToggleNowLine from "./ToggleNowLine.vue";
import SettingsButton from "./SettingsButton.vue";
import { useMarkwhenStore } from "@/Markwhen/markwhenStore";
import ToggleDateTimeDisplay from "./ToggleDateTimeDisplay.vue";
import ToggleShowProgress from "./ToggleShowProgress.vue";
import UserRanges from "./UserRanges.vue";

const timelineStore = useTimelineStore();
const markwhenStore = useMarkwhenStore();

const styleLeftInset = computed(() => {
  return timelineStore.pageSettings.viewport.offsetLeft;
});

const copyToClipboard = async (s: string, description?: string) => {
  if (!navigator.clipboard) {
    return alert("Clipboard not available :/");
  }
  try {
    await navigator.clipboard.writeText(s);
    alert(`Copied ${description}to clipboard.`);
  } catch (e) {
    console.error(e);
    alert("Unable to copy to clipboard:" + e);
  }
};

const copyTimelineLink = async () =>
  copyToClipboard(markwhenStore.timelineLink, "link ");

const copyEmbedLink = async () =>
  copyToClipboard(markwhenStore.embedLink, "embed code ");

const goToNow = () => timelineStore.goToNow();
</script>

<template>
  <div
    class="fixed hover:text-slate-700 dark:text-slate-400 text-slate-500 dark:hover:text-slate-300 flex-items-center justify-center"
    :style="`left: calc(${styleLeftInset}px); bottom: 0rem; right: 0;`"
  >
    <div
      class="flex flex-row items-center justify-center overflow-scroll noScrollBar relative w-full h-full pt-20"
    >
      <div class="flex flex-row rounded m-3">
        <div class="dark:bg-slate-700 bg-white flex flex-row gap-2 px-2 rounded p-0.5">
          <div
            class="flex flex row overflow-visible p-[2px] pointer-events-auto"
            style="grid-area: gantt"
          >
            <ToggleMode></ToggleMode>
          </div>
          <div class="flex flex-row pointer-events-auto shrink-0">
            <SettingsButton class="gap-1" @click="goToNow">
              <svg
                class="h-4 w-4"
                focusable="false"
                aria-hidden="true"
                viewBox="0 0 24 24"
                data-testid="ModeStandbyIcon"
                fill="currentColor"
                stroke="currentColor"
                stroke-width="0.4"
              >
                <path
                  d="M12 2C6.49 2 2 6.49 2 12s4.49 10 10 10 10-4.49 10-10S17.51 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8zm3-8c0 1.66-1.34 3-3 3s-3-1.34-3-3 1.34-3 3-3 3 1.34 3 3z"
                ></path></svg
              ><span class="text-xs mt-px">Now</span>
            </SettingsButton>
            <UserRanges></UserRanges>
            <AutoCenter></AutoCenter>
          </div>
          <div
            class="flex flex row overflow-visible p-[2px] pointer-events-auto"
          >
            <!-- <ToggleMiniMap></ToggleMiniMap> -->
            <TimelineScale></TimelineScale>
          </div>
          <div
            class="overflow-visible p-[2px] pointer-events-auto flex flex-row"
          >
            <ToggleDateTimeDisplay></ToggleDateTimeDisplay>
            <ToggleShowProgress></ToggleShowProgress>
          </div>
          <div
            class="overflow-visible p-[2px] pointer-events-auto flex flex-row"
          >
            <ExpandAll></ExpandAll>
            <CollapseAll></CollapseAll>
            <ToggleNowLine></ToggleNowLine>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
