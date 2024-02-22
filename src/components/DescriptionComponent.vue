<script setup>
const originData = ref([
  {
    url: 'https://event.cw.com.tw/_test-by-cwlab/doltc/slide1.jpg',
    title: '面對照顧壓力大？ 張曼娟：',
    title2: '記得提醒自己，這一切都會過去的',
    description:
      '各位照顧者又要展開新的一年，每天闖關又常感到孤單的日子。\n所幸，我們並不寂寞，我們在這條艱難的路上遇見彼此。'
  },
  {
    url: 'https://event.cw.com.tw/_test-by-cwlab/doltc/slide2.jpg',
    title: '小六生可能已是照顧者',
    title2: '日本小學生與失智者共開店',
    description:
      '當我們擔心學童與失智者接觸的安危，日本小學已納入課程，把長者視為社區的一份子。你我都可能是照護者，共生才能撐起高齡長照。。'
  },
  {
    url: 'https://event.cw.com.tw/_test-by-cwlab/doltc/slide3.jpg',
    title: '不想活得又老又窮',
    title2: '避開50歲最後悔的5個財務決定',
    description:
      '「早知道會活那麼久，當初就……」想避免退休後，活得又老又窮，盡量避開這些50歲以上的人最後悔的財務決定。'
  }
]);
const copyData = ref([]);
const clickWait = ref(false);
const slideCtrl = (slidesToShow = 1) => {
  if (clickWait.value) {
    return;
  }
  stopTimeLoop();
  clearClickWaitTimer();

  clickWait.value = true;

  const spliceItem = copyData.value.splice(0, slidesToShow);
  copyData.value.push(...spliceItem);

  setClickWait();
  setTimeLoop();
};

const timer = ref(null);
const setClickWait = () => {
  timer.value = setTimeout(() => {
    clickWait.value = false;
  }, 500);
};

const clearClickWaitTimer = () => {
  clearInterval(timer.value);
};

const timeLoop = ref(null);
const setTimeLoop = () => {
  timeLoop.value = setInterval(() => {
    slideCtrl(1);
  }, 5000);
};

const stopTimeLoop = () => {
  clearInterval(timeLoop.value);
};

const selectImage = (index) => {
  if (clickWait.value) {
    return;
  }

  const currentRef = copyData.value[0].ref;

  if (currentRef === index) {
    return;
  }

  if (
    (currentRef === 0 && index === 2) ||
    (currentRef === 1 && index === 0) ||
    (currentRef === 2 && index === 1)
  ) {
    slideCtrl(2);
  } else {
    slideCtrl(1);
  }
};

onMounted(() => {
  for (let i = 0; i < originData.value.length * 3; i += 1) {
    const obj = {};
    obj.id = i;
    obj.ref = i % originData.value.length;
    copyData.value.push(obj);
  }
  setTimeLoop();
});

onUnmounted(() => {
  stopTimeLoop();
});
</script>
<template>
  <main class="2xl:container 2xl:mx-auto" id="description">
    <div
      class="py-[56px] pl-[24px] md:mt-[30px] lg:pl-[40px] md:pl-[40px] md:pt-[70px] xl:pl-[40px]"
    >
      <div class="lg:ml-[104px] md:ml-[80px] xl:ml-[180px] md:flex md:items-start md:justify-start">
        <div class="mr-[24px] hidden lg:block">
          <p
            class="relative mt-[9px] text-[12px] text-primary font-semibold tracking-[0.225rem] write-vertical-left"
          >
            LTCS LIVE<span class="absolute left-0 top-0 h-[15px] w-[1px]">
              <span class="absolute left-0 top-0 h-[11px] w-[1px] bg-secondary" />
              <span class="absolute left-0 top-[15px] h-[4px] w-[1px] bg-secondary" />
            </span>
          </p>
        </div>
        <div>
          <h2 class="text-[32px] text-content font-semibold leading-[42px] tracking-[0.4rem]">
            照顧現場
          </h2>
          <h3 class="text-[32px] text-primary font-semibold leading-[42px] tracking-[0.4rem]">
            預見安心變老
          </h3>
          <p class="relative text-[12px] text-primary font-semibold tracking-[0.225rem] lg:hidden">
            LTCS LIVE<span class="absolute bottom-0 left-0 h-[1px] w-full">
              <span class="absolute left-0 top-0 h-[1px] w-[11px] bg-secondary" />
              <span class="absolute left-[15px] top-0 h-[1px] w-[4px] bg-secondary" />
            </span>
          </p>
        </div>
      </div>
      <div class="relative mt-[24px] overflow-hidden rounded-tl-[40px] md:mt-[32px]">
        <button
          type="button"
          class="bg-main justufy-center absolute right-[40px] top-1/2 z-[11] h-[32px] w-[32px] flex items-center lg:right-[200px] md:right-[80px] md:h-auto md:w-auto xs:h-[48px] xs:w-[48px]"
          @click="slideCtrl(1)"
        >
          <img src="../assets/images/slidebutton.png" class="block h-auto max-w-full" alt="" />
        </button>
        <TransitionGroup name="flip-list" tag="ul" class="flex">
          <li
            v-for="(item, index) in copyData"
            :key="item"
            class="carousel-items relative left-[calc(-322px*3)] lg:left-[calc(-900px*3)] md:left-[calc(-700px*3)] sm:left-[calc(-600px*3)] xl:left-[calc(-955px*3)] xs:left-[calc(-500px*3)]"
          >
            <div
              class="relative h-auto w-[322px] lg:w-[900px] md:w-[700px] sm:w-[600px] xl:w-[955px] xs:w-[500px]"
            >
              <div class="mask absolute left-0 top-0 z-[1] h-full w-full"></div>
              <img class="max-w-full object-cover" :src="originData[item.ref].url" alt="" />
            </div>
            <div class="relative hidden md:block">
              <div
                class="absolute bottom-[50px] left-[50px] z-[10] lg:bottom-[76px] lg:left-[129px] xl:bottom-[107px] xl:left-[180px]"
                v-if="index === 3"
              >
                <h3
                  class="text-white font-semibold md:text-[28px] xl:text-[32px] md:leading-[39.2px] xl:leading-[44.8px]"
                >
                  {{ originData[item.ref]?.title }}
                </h3>
                <h3
                  class="text-white font-semibold md:text-[28px] xl:text-[32px] md:leading-[39.2px] xl:leading-[44.8px]"
                >
                  {{ originData[item.ref]?.title2 }}
                </h3>
                <p
                  class="w-1/2 truncate text-ellipsis text-[16px] text-white font-normal leading-[25.6px] tracking-[1.6px]"
                >
                  {{ originData[item.ref]?.description }}
                </p>
                <img src="../assets/images/rectangle.svg" class="mt-[24px]" alt="" />
              </div>
            </div>
          </li>
        </TransitionGroup>
        <div
          class="absolute bottom-[30px] left-[50px] z-[15] mt-[16px] hidden w-[48px] lg:left-[128px] xl:bottom-[59px] xl:left-[180px] md:mt-[40px] md:flex sm:justify-between"
        >
          <div
            v-for="(item, index) in originData"
            :key="index"
            class="pointer-events-auto h-[8px] w-[8px] border-[1px] rounded-full"
            :class="
              copyData[0]?.ref === index
                ? 'bg-secondary border-[#CADEDF00]'
                : 'bg-[#00000000] border-[#CADEDF]'
            "
            @click="selectImage(index)"
          ></div>
        </div>
      </div>
      <template v-for="(item, index) in copyData" :key="item">
        <div
          class="mt-16px h-auto w-[322px] md:hidden lg:w-[900px] md:w-[700px] sm:w-[600px] xl:w-[955px] xs:w-[500px]"
          v-if="index === 3"
        >
          <h3 class="text-block text-[20px] font-semibold leading-[28px]">
            {{ originData[item.ref]?.title }}
          </h3>
          <h3 class="text-block text-[20px] font-semibold leading-[28px]">
            {{ originData[item.ref]?.title2 }}
          </h3>
          <p
            class="text-block line-clamp-2 whitespace-pre-wrap text-[14px] font-normal leading-[25.6px] tracking-[1.6px] xs:line-clamp-1"
          >
            {{ originData[item.ref]?.description }}
          </p>
          <img src="../assets/images/rectangle.svg" class="mt-[16px]" alt="" />
        </div>
      </template>
      <div class="z-[15] mt-[16px] w-[48px] flex justify-between md:hidden">
        <div
          v-for="(item, index) in originData"
          :key="index"
          class="pointer-events-auto h-[8px] w-[8px] border-[1px] rounded-full"
          :class="
            copyData[0]?.ref === index
              ? 'bg-secondary border-[#CADEDF00]'
              : 'bg-[#00000000] border-[#CADEDF]'
          "
          @click="selectImage(index)"
        ></div>
      </div>
    </div>
  </main>
</template>
<style lang="scss" scoped>
//* {
//outline: 1px solid #000;
//}

.mask {
  opacity: 0.8;
  background: linear-gradient(to top, #000 0%, #00000000 40%);
}
.writing-mode {
  -webkit-writing-mode: vertical-lr;
  writing-mode: vertical-lr;
}
.flip-list-move {
  transition: transform 0.8s;
}
.text {
  opacity: 0;
  z-index: -5;
}
.carousel-items {
  opacity: 0;
  z-index: -3;
}
.carousel-items:nth-child(2) {
  opacity: 1;
  z-index: -2;
}
.carousel-items:nth-child(3) {
  opacity: 1;
  z-index: -1;
}
.carousel-items:nth-child(4) {
  opacity: 1;
  z-index: 1;
}
.carousel-items:nth-child(5) {
  opacity: 1;
  z-index: 2;
}
</style>
