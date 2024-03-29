<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useAuthStore } from '../stores/auth';
import dayjs from 'dayjs';
import 'dayjs/locale/ko';
import relativeTime from 'dayjs/plugin/relativeTime';
dayjs.extend(relativeTime);
const authStore = useAuthStore();
const streamInfo = ref<typeof authStore.streamInfo>();

onMounted(async () => {
  streamInfo.value = await authStore.getStreamInfo();
});
</script>
<template>
  <div class="snap-x mb-12 flex gap-6 w-full overflow-auto snap-mandatory">
    <a
      v-if="streamInfo?.status !== 'offline' && streamInfo?.status !== 'unknown'"
      href="/live"
      target="_blank"
      class="flex flex-col justify-between snap-always shrink-0 snap-start w-full max-w-[280px] sm:max-w-[304px] rounded-3xl p-6 transition-colors duration-300 ease-in-out"
      :class="{
        'bg-blue-200 hover:bg-blue-300 dark:bg-blue-800 dark:hover:bg-blue-700': streamInfo?.status === 'online',
        'bg-slate-100 dark:bg-neutral-800': streamInfo?.status !== 'online',
      }"
    >
      <span
        class="line-clamp-2 text-xl text-black dark:text-slate-200"
      >
        {{
          streamInfo?.status === "online"
            ? streamInfo?.title
            : "불러오는 중..."
        }}
      </span>
      <div v-if="streamInfo?.status === 'online'">
        <div class="line-clamp-1 text-black dark:text-slate-200">
          {{ streamInfo?.game_name }}
        </div>
        <div class="text-black dark:text-slate-200">{{ dayjs().locale("ko").to(dayjs(streamInfo?.started_at)) }}</div>
      </div>
    </a>
    <div
      v-else
      class="flex flex-col justify-between snap-always shrink-0 snap-start w-full h-44 max-w-[280px] sm:max-w-[304px] rounded-3xl p-6 bg-slate-100 dark:bg-neutral-800"
    >
      <span class="icon xi-x xi-snooze mt-1 mb-3 text-dark dark:text-slate-200" />
      <div class="text-xl text-dark dark:text-slate-200 mb-3">
        방송이 꺼져있을 때 ⋯
      </div>
      <div class="flex w-full gap-3 text-sm">
        <a
          target="_blank"
          href="https://tgd.kr/s/naseongkim"
          class="w-1/2 text-center rounded-2xl bg-twitch-purple dark:hover:bg-violet-600 py-3 text-white transition-colors duration-300 eaes-in-out hover:bg-violet-600"
        >
          트게더
        </a>
        <a
          target="_blank"
          href="https://www.youtube.com/channel/UCfLvxrf3KoKpUG0bBHIZJ-g"
          class="w-1/2 text-center rounded-2xl bg-slate-700 py-3 transition-colors duration-300 eaes-in-out hover:bg-slate-600 text-white"
        >
          방송 다시보기
        </a>
      </div>
    </div>
    <a
      href="/get"
      class="snap-always shrink-0 snap-start w-full max-w-[280px] sm:max-w-[304px] h-44 flex flex-col justify-between rounded-3xl p-6 transition-all duration-300 ease-in-out bg-green-50 hover:bg-green-100 dark:bg-green-900/50 hover:dark:bg-green-900"
    >
      <span class="icon xi-x xi-scissors mt-1 mb-6 text-dark dark:text-slate-200" />
      <div class="text-xl text-dark dark:text-slate-200">
        새로운 클립<br>만들기 →
      </div>
    </a>
    <a
      traget="_blank"
      href="https://next.naseong.kim"
      class="snap-always shrink-0 snap-start w-full max-w-[280px] sm:max-w-[304px] h-44 flex flex-col justify-between rounded-3xl p-6 transition-all duration-300 ease-in-out bg-black/10 hover:bg-black/20 dark:bg-neutral-950 hover:dark:bg-neutral-950/50"
    >
      <span class="icon xi-x xi-external-link mt-1 mb-6 text-dark dark:text-slate-200" />
      <div class="text-xl text-dark dark:text-slate-200">
        완전히 새롭게,<br>다시 만드는 나클립 →
      </div>
    </a>
    <a
      href="/import"
      class="snap-always shrink-0 snap-start w-full max-w-[280px] sm:max-w-[304px] h-44 flex flex-col justify-between rounded-3xl p-6 transition-all duration-300 ease-in-out bg-red-50 hover:bg-red-100 dark:bg-red-900/50 hover:dark:bg-red-900"
    >
      <span class="icon xi-x xi-file-video-o mt-1 mb-6 text-dark dark:text-slate-200" />
      <div class="text-xl text-dark dark:text-slate-200">
        유튜브 다시보기에서<br>클립을 만들어 보세요 →
      </div>
    </a>
    <a
      target="_blank"
      href="https://naseongkim.channel.io"
      class="snap-always shrink-0 snap-start w-full max-w-[280px] sm:max-w-[304px] h-44 flex flex-col justify-between rounded-3xl p-6 transition-all duration-300 ease-in-out bg-violet-50 hover:bg-violet-100 dark:bg-violet-900/50 hover:dark:bg-violet-900"
    >
      <span class="icon xi-x xi-forum-o mt-1 mb-6 text-dark dark:text-slate-200" />
      <div class="text-xl text-dark dark:text-slate-200">
        개발자에게<br>의견 보내기 →
      </div>
    </a>
  </div>
</template>
