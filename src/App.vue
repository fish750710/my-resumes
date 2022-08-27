<template>
  <div>
    <div class="header w-full h-12 flex justify-end fixed text-white">
      <el-tabs v-model="navActive" class="demo-tabs" @tab-click="handleClick" v-for="(item, index) in state.headerData"
        :key="index">
        <el-tab-pane :label="item.name" :name="item.id"></el-tab-pane>
      </el-tabs>
    </div>
    <div class="content w-full">
      <div
        class="banner flex flex-col items-center section-box h-[30rem] sm:h-[30rem] md:h-[40rem] lg:h-[50rem] xl:h-[60rem] 2xl:h-[68rem]">
        <img src="@/assets/images/banner.jpg" alt="">
        <div id="intro" class="intro">
          <div class="headshot"></div>
          <div class="personal">
            <div class="name">
              <font-awesome-icon icon="fa-solid fa-user-secret" />
              {{ state.introData.name }}
            </div>
            <div class="en-name">
              <font-awesome-icon icon="fa-solid fa-user" />
              {{ state.introData.nameEn }}
            </div>
            <div class="tel">
              <a :href="'tel:' + state.introData.tel">
                <font-awesome-icon icon="fa-solid fa-phone" />
                {{ state.introData.tel }}
              </a>
            </div>
            <div class="mail">
              <a :href="'mailto:' + state.introData.tel">
                <font-awesome-icon icon="fa-solid fa-at" />
                {{ state.introData.mail }}
              </a>
            </div>
            <div class="line">
              <a href="https://line.me/ti/p/qoCyMUBsgj" target="_bark">
                <font-awesome-icon icon="fa-solid fa-comment-dots" />
                {{ state.introData.line }}
              </a>
              <a href="https://line.me/ti/p/qoCyMUBsgj" target="_bark">
                <img src="https://scdn.line-apps.com/n/line_add_friends/btn/zh-Hant.png" alt="加入好友" height="20"
                  border="0">
              </a>
            </div>
            <div class="github">
              <a :href="state.introData.github" target="_bark">
                <font-awesome-icon icon="fa-brands fa-github" />
                fish750710
              </a>
            </div>
          </div>
        </div>
      </div>
      <div id="skill" class="skill-content section-box">
        <div class="title fade-in">
          <font-awesome-icon icon="fa-solid fa-kitchen-set" />
          技能
        </div>
        <!-- <ul>
          <li class="w-60" v-for="(item, index) in state.skillData" :key="index">
            <h3>{{ item.name }}</h3>
            <span>{{ item.description }}</span>
          </li>
        </ul> -->
        <div class="skill-content__box">
          <el-collapse v-model="skillCollapse" class="skill-content__ul" accordion>
            <el-collapse-item :title="item.name" :name="index" class="skill-content__li w-80 md:w-[18rem] xl:w-80"
              v-for="(item, index) in state.skillData" :key="index">
              <!-- <h3>{{ item.name }}</h3> -->
              <span>{{ item.description }}</span>
            </el-collapse-item>
          </el-collapse>
        </div>
      </div>
      <div id="experience" class="experience-content section-box">
        <div class="title fade-in">
          <font-awesome-icon icon="fa-solid fa-star" />
          公司經歷
        </div>
        <ul class="company-box">
          <li class="company-li w-4/5" v-for="(item, index) in state.experience" :key="index">
            <h3 class="">
              <img :src="item.logo" alt="">
              <span>{{ item.companyName }}</span>
            </h3>
            <h4>{{ item.jobName }} <span>{{ item.date }}</span></h4>
            <span class="description">{{ item.description }}</span>
            <div class="place" v-if="item.place">駐點於: {{ item.place }}</div>
            <el-collapse v-model="companyCollapse" v-for="(objArr, key) in item.object" :key="key">
              <font-awesome-icon icon="fa-solid fa-briefcase" />
              <el-collapse-item :title="objArr.objectName" :name="objArr.id">
                <ul>
                  <li v-for="(list, key) in objArr.items" :key="key">
                    {{ list }}
                  </li>
                </ul>
              </el-collapse-item>
            </el-collapse>
            <div class="skill">
              <p v-for="val in item.skill" :key="val">#{{ val }}</p>
            </div>
          </li>
        </ul>
      </div>
      <div id="about" class="about-content section-box">
        <div class="title" style="color: #fff;" :class="[navActive === 'about' ? 'fade-in' : 'fade-out']">
          <font-awesome-icon icon="fa-solid fa-address-card" />
          關於我
        </div>
        <!-- <div class="bg w-72 h-60" :style="[navActive === 'about' ? 'opacity: 0' : 'opacity: 1']"></div> -->
        <p v-html="state.about" :class="[navActive === 'about' ? 'fade-in' : 'fade-out']"></p>
      </div>
      <div id="portfolio" class="portfolio-content section-box">
        <div class="title fade-in">
          <font-awesome-icon icon="fa-solid fa-file" />
          作品
        </div>
        <ul class="portfolio-box">
          <li v-for="(item, index) in state.portfolio" :key="index" :class="{ 'jitter': state.bounceIndex === index }">
            <div class="card w-[18rem]">
              <a :href="item.url" target="_blank">
                <img :src="item.img" :alt="item.title">
              </a>
              <div class="sub-title">{{ item.title }}</div>
              <div class="description" v-html="item.description"></div>
              <main>
                <ul class="info">
                  <li v-for="(list, key) in item.info" :key="key">
                    <font-awesome-icon icon="fa-solid fa-circle-check" />
                    <p>{{ list }}</p>
                  </li>
                </ul>
                <ul class="text">
                  <li v-for="text in item.text" :key="text" v-html="text">
                  </li>
                </ul>
                <ul class="skill">
                  <li v-for="val in item.skill" :key="val">
                    <p>{{ val }}</p>
                  </li>
                </ul>
              </main>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="footer">
      <p>{{ state.footer }}</p>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, onMounted, nextTick, onUnmounted } from 'vue';

const state = reactive({
  headerData: [
    { id: 'intro', name: '聯絡資訊' },
    { id: 'skill', name: '技能' },
    { id: 'experience', name: '公司經歷' },
    { id: 'about', name: '關於我' },
    { id: 'portfolio', name: '作品' },
  ],
  introData: {
    name: '吳存鴻',
    nameEn: 'Hans',
    tel: '0988-737-467',
    mail: 'fish750710@gmail.com',
    line: 'fish750710',
    github: 'https://github.com/fish750710',
  },
  skillData: [
    {
      name: 'Vue.js',
      description: 'Vue2 & Vue3 & Vuex，開發SPA架構網站',
    },
    {
      name: 'Nuxt.js',
      description: '建構 SSR 架構網站',
    },
    {
      name: 'Sass 和 Tailwind',
      description: '規劃 RWD版型，模組化 CSS',
    },
    {
      name: 'Bootstrap、Element UI',
      description: '開發後台介面',
    },
    {
      name: 'jQuery',
      description: '快速開發',
    },
    {
      name: 'WebSocket',
      description: '開發即時通訊和資料傳遞',
    },
    {
      name: 'PL/SQL、MySQL',
      description: '存取資料庫',
    },
    {
      name: 'Java',
      description: '開發後台',
    },
    {
      name: 'Echarts、Highcharts',
      description: '開發動態圖表',
    },
    {
      name: 'Canvas、Phaser.js',
      description: '開發小遊戲和繪製相關',
    },
    {
      name: 'git',
      description: '管理版本控制',
    },
    {
      name: 'node、Express.js',
      description: '建立server，在前端與後端的溝通時處理API和商業邏輯',
    },
    {
      name: 'i18n',
      description: '支援多語系',
    },
    {
      name: 'TypeScript',
      description: '嚴格執行型別檢查，避免不必要的錯誤',
    },
  ],
  experience: [
    {
      companyName: '昊盈資訊有限公司',
      logo: require('@/assets/images/company/skyfill.jpg'),
      jobName: '前端工程師',
      date: '2021/1 - 2022/2',
      place: '',
      description: '由盈德網絡資訊部獨立出來的新公司。',
      skill: ['Vue.js', 'JavaScript', 'Sass', 'HTML', 'Git', 'SSI', 'Swiper', 'AJAX', 'Highcharts', 'wow.js', 'GA', 'TypeScirpt', 'WebSocket', 'EChart', 'ESlint'],
      object: [
        {
          id: '006',
          objectName: 'H5 Web交易平台',
          items: [
            '開發功能與 App 接近的 H5 交易平台網站。',
            '建置公版交易網站，提供其他事業部使用。',
            '新專案導入 Vue3 + TypeScript。',
            '使用 Vue3 + Vuex + WebSocket 技術開發。',
            '使用 Chart.js 開發動態圖表。',
            '建置模組化，及 component 化。',
            '後端討論 API 串接與測試。',
            '跨部門討論與合作。與策劃、PM、UI 設計討論功能需求，設計方向與討論，持續優化 UX。',
          ]
        },
        {
          id: '005',
          objectName: 'H5線上文字直播功能',
          items: [
            'Vue2 + Vuex + WebSocket 開發，後續功能優化及升級 Vue3。',
            '建置模組化，及 component 化。',
            '後端討論 API 串接與測試。',
            'PM 討論功能需求，設計方向與討論。',
            'UI / UX設計討論可行性與回饋。',
            'App 對接功能與測試。',
          ]
        }
      ]
    },
    {
      companyName: '盈德網絡服務有限公司 ',
      logo: require('@/assets/images/company/itnsl.jpg'),
      jobName: '前端工程師',
      date: '2020/1 - 2020/12',
      place: '',
      description: '官網、M 站、H5 及活動相關頁面的維護及開發。',
      skill: ['Vue.js', 'Vuex', 'JavaScript', 'jQuery', 'Sass', 'HTML', 'Git', 'SSI', 'Swiper', 'AJAX', 'Highcharts', 'wow.js', 'GA', 'PHP', 'React.js'],
      object: [
        {
          id: '004',
          objectName: 'landing page、活動頁面',
          items: [
            '根據設計稿切版。',
            '設計符合 RWD 功能。',
            '協助處理 Bug 及優化。',
            '串接相關 API。',
            '簡易動畫設計。',
            'git 版控。',
          ]
        },
        {
          id: '003',
          objectName: 'H5 線上文字直播功能',
          items: [
            'Vue2 + Vuex + WebSocket 開發，後續功能優化及升級 Vue3。',
            '建置模組化，及 component 化。',
            '後端討論 API 串接與測試。',
            'PM討論功能需求，設計方向與討論。',
            'UI / UX 設計討論可行性與回饋。',
            'App 對接功能與測試。',
          ]
        },
        {
          id: '002',
          objectName: '開發動態圖表頁面',
          items: [
            '既有框架下使用 new Vue 動態綁定資料。',
            '串接 API 處理資料。',
            '使用 Highcharts 圖表套件。',
          ]
        },
        {
          id: '001',
          objectName: '小遊戲開發',
          items: [
            '串接 API 報名功能。',
            '設計不同難易度關卡。',
            '使用 Proxy 控制資料及畫面。(打地鼠)',
            '使用物件導向開發，原型鍊方式搭配 canvas 開發。(口紅機)',
            '使用 Phaser3 遊戲引擎開發。(下樓梯)',
          ]
        },
      ]
    },
    {
      companyName: '碩誠國際股份有限公司',
      logo: require('@/assets/images/company/serlinkin.png'),
      jobName: '軟體設計工程師',
      date: '2019/1 - 2020/1',
      place: '新光人壽',
      description: '駐點於新光人壽維護與升級資料庫系統:維護現有資料庫(續期)，優化及改善資料庫功能，底層重構，(續期)和改善網頁功能。',
      skill: ['PL-SQL', 'JAVA', 'HTML', 'JavaScript', 'ClearCase', 'Oracle', 'JDeveloper'],
    },
    {
      companyName: '碩誠國際股份有限公司',
      logo: require('@/assets/images/company/serlinkin.png'),
      jobName: '軟體設計工程師',
      date: '2017/7 - 2018/12',
      place: '台北富邦',
      description: '駐點於台北富邦銀行外匯系統交易專案:開發銀行內部新外匯交易系統(貿融、光票)與WEB頁面，進行除錯及優化交易，協助測試交易。',
      skill: ['JAVA', 'Git', 'OracleDB'],
    },
  ],
  about: `【工作歷程】<br /><br />
          JAVA軟體工程師經驗兩年，負責開發銀行內部外匯系統專案。工作之餘自我進修WEB相關課程，藉此更激發了我對網頁開發的興趣。
          前端工程師經驗三年，負責官網和後台功能開發與維護。熟悉HTML5、CSS3、JavaScript、jQuery、Bootstrap4、git等，可獨立切版、完成RWD網頁及開發完整功能的SPA網站。專長使用 Vue 2、3 、Nuxt 開發 SPA網站。工作之餘自我進修 TypeScript 和React.js。
          <br /><br />
          【未來期許】<br /><br />
          軟體資訊業一直都是台灣的軟實力，我願意將過去在別的行業累積的經驗及對解決顧客問題的熱情，再度發揮在編寫軟體上。希望能有機會發揮所長為貴公司奮鬥，並與其他工程師互相學習成長，如有機會我也會利用閒暇時間再度進修，精益求精並幫助公司創造績效。`,
  portfolio: [
    {
      img: require('@/assets/images/projects/vue3_trade.jpg'),
      title: 'H5 Web UI 交易平台',
      skill: ['Vue3', 'Vuex', 'TypeScript', 'eChart', 'ESLint'],
      description: `獨立完成專案開發。共用元件 component化。
                    全新後端服務器串接，技術算計算。誇部門討論和合作，隨時調整修改。`,
      info: [
        'Web UI 大改版，提供更多功能及優化效能提高速度。',
        '提供接近APP體驗的平台，使用戶能快速上手。',
        '除了APP和PC以外，提供用戶另外的選擇。提供多圖比對功能(APP無)。',
        '提供深夜模式選擇。',
        '支援圖表類型調整及技術線客製化。',
        '自選及編輯功能。',
        '會員登錄和帳戶切換功能。',
      ],
      url: 'https://78hqar.com/',
      text: ['專案部分功能需登錄才能瀏覽，如需登錄請與我聯絡。',
        '平台有限制地區瀏覽',
        '<a href="https://www.youtube.com/watch?v=iglaZaUIepY" target="_blank" style="color: red;">操作影片連結</ a>'
      ],
    },
    {
      img: require('@/assets/images/projects/vue3_chat.jpg'),
      title: 'CMS 文字直播',
      skill: ['Vue3', 'Vuex', 'TypeScript'],
      description: `獨立完成專案開發。共用元件 component化。
                    與後端對接串接API與socket。
                    用戶反應熱烈，即時與分析師討論情勢與行情，提供用戶問與答。`,
      info: [
        '全新功能開發，APP使用H5頁面。',
        '分析師與用戶即時互動。',
        '用戶留言板問與答。',
        '分析師分享走勢。',
      ],
      url: 'https://h5.bxcfd.com/circle/circle/#/Viewpoint',
      text: ['此專案部分功能(直播和我的問答)必須登錄才能瀏覽，如需登錄請與我聯絡。',
        '此頁面有限制地區瀏覽。',
      ],
    },
    {
      img: require('@/assets/images/projects/nuxt_3c.jpg'),
      title: '電商網站(作品)',
      skill: ['Nuxt2', 'Axios', 'vee-validate3', 'Swiper','animate'],
      description: `由原本電商網站Vue 2 改寫成 Nuxt 版本。`,
      info: [
        '支援RWD效果。',
        '上次未登出，會自動儲存登入狀態。',
        '輪播功能。',
        '儲存最愛到 Local Storage。',
        '商品資料由後台動態產生。',
        '會員登入、購物車、喜愛商品、搜尋商品、商品分類，價格排序、優惠碼折扣功能。',
        '訂單完成傳送至後台後，產生相關訂單資訊 (需登入)。',
        '後台-商品、優惠碼新增、修改、刪除等功能。',
      ],
      url: 'https://fish750710.github.io/GoldStore-Nuxt-static/',
      github: 'https://github.com/fish750710/GoldStore-Nuxt',
    },
    {
      img: require('@/assets/images/projects/game1.jpg'),
      title: '射拐杖糖小遊戲',
      description: '配合聖誕節節日開發符合需求小遊戲。<br /><br /> 用戶反應熱烈，點擊率高，營收比上個月提升30%。',
      skill: ['CANVAS', 'JavaScript'],
      info: ['共三關，從易到難，過關可領取不同獎勵。', '會員登錄功能。', '支援RWD功能。'],
      url: 'https://fish750710.github.io/game1/game1.html'
    },
    {
      img: require('@/assets/images/projects/game2.jpg'),
      title: '打地鼠遊戲',
      skill: ['JavaScript', 'jQuery'],
      description: '開發符合需求小遊戲。 <br /><br />用戶反應熱烈，點擊率高，營收比上個月提升18%。',
      info: ['使用Proxy改變資料狀態，來控制DOM元素。', '隨著時間倒數，速度越來越快。', '支援RWD功能。'],
      url: 'https://fish750710.github.io/game2/'
    },
    {
      img: require('@/assets/images/projects/game3.jpg'),
      title: '下樓梯小遊戲',
      skill: ['Phaser 3', 'JavaScript'],
      description: '配合過年節日開發符合需求小遊戲。 <br /><br />用戶反應熱烈，點擊率高，營收比上個月提升20%。',
      info: ['共三關，從易到難，過關可領取不同獎勵。', '會員登錄功能。', '支援RWD功能。'],
      url: 'https://fish750710.github.io/game3/'
    },
    {
      img: require('@/assets/images/projects/game4.jpg'),
      title: '離線小恐龍遊戲',
      skill: ['CANVAS', 'JavaScript'],
      description: '開發符合需求小遊戲。 <br /><br />用戶反應熱烈，點擊率高，營收比上個月提升15%。',
      info: ['使用 JavaScript portotype。', '隨著時間速度越來越快，產生的障礙物會更多。', '支援RWD功能。'],
      url: 'https://fish750710.github.io/game4/'
    },
    {
      img: require('@/assets/images/projects/vue2_3c.jpg'),
      title: '電商網站(作品)',
      skill: ['Vue2', 'Vuex', 'Axios', 'vee-validate3', 'animate', 'BootStrap4', 'jQuery', 'Swiper', 'vue-piczoom', 'ESlint'],
      description: ``,
      info: [
        '支援RWD效果。',
        '上次未登出，會自動儲存登入狀態。',
        '輪播功能。',
        '儲存最愛到 Local Storage。',
        '商品資料由後台動態產生。',
        '後台-商品、訂單狀態、優惠，新增修改刪除等功能 (需登入)',
        '前台-會員登入、購物車、喜愛商品、搜尋商品、商品分類，價格排序、優惠碼折扣功能',
        '訂單完成傳送至後台後，產生相關訂單資訊 (需登入)',
        '根據內容數量產生動態分頁',
        '商品圖片放大鏡功能',
      ],
      url: 'https://fish750710.github.io/Gold-Store/',
      github: 'https://github.com/fish750710/GoldStore',
    },
    {
      img: require('@/assets/images/projects/bigdata.jpg'),
      title: '數據圖表',
      skill: ['Vue2', 'highcharts','scss', 'jQuery'],
      description: `串接 API 動態產生客製化圖表`,
      info: [
        '使用new Vue 動態綁定資料。',
        'highcharts.js 動態呈現圖表。',
        '客製化圖表。',
        '數據來源串接API。',
        '支援RWD自適應。',
      ],
      url: 'https://fish750710.github.io/highcharts/',
      github: '',
    },
    {
      img: require('@/assets/images/projects/nonfarm.jpg'),
      title: 'loading Page',
      skill: ['Vue2', 'highcharts', 'scss', 'jQuery', 'Swiper'],
      description: `串接 API 產生動態資料與圖表`,
      info: [
        '使用new Vue 動態綁定資料。',
        'highcharts.js 動態呈現圖表。',
        '客製化圖表。',
        '數據來源串接API。',
        '支援RWD自適應。',
        'Swiper效果',
      ],
      url: 'https://fish750710.github.io/nonfarm/index.html',
      github: '',
    },
    {
      img: require('@/assets/images/projects/restaurant.jpg'),
      title: '老饕餐館',
      skill: ['JavaScript', 'scss', 'jQuery', 'Swiper'],
      description: `形象官網`,
      info: [
        '靜態頁面。',
        '支援RWD自適應。',
        'Swiper效果',
      ],
      url: 'https://fish750710.github.io/RWD-Restaurant',
      github: '',
    },
  ],
  footer: 'Copyright © Hans Wu All rights reserved.',
  timer: null,
  bounceIndex: 0,
})

const navActive = ref('intro')
const skillCollapse = ref('')
const companyCollapse = ref('')

function handleClick(tab, e) {
  const id = tab.props.name;
  document.getElementById(id).scrollIntoView() - 50;
}
function handleScroll() {
  const scrollPos = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop;
  const sectionList = sectionLiArr.value;
  for (let i = 0; i <= sectionList.length; i++) {
    if (scrollPos > sectionList[i] - 300) {
      navActive.value = state.headerData[i].id;
    }
  }
}
const sectionLiArr = ref([]);
function getSectionPos() {
  nextTick(() => {
    const sectionLi = document.querySelectorAll('.section-box');
    // console.log(sectionLi, sectionLi[0].clientWidth)
    // console.log(sectionLi[1].offsetTop)
    sectionLi.forEach((li) => {
      // console.log(li.getBoundingClientRect().top)
      return sectionLiArr.value.push(li.getBoundingClientRect().top + window.scrollY);
    });
  })
}

function getRandom() {
  const fileLen = state.portfolio.length;
  state.timer = setInterval(() => {
    state.bounceIndex = Math.floor(Math.random() * fileLen);
  }, 3500);
}

onMounted(() => {
  addEventListener('scroll', handleScroll)
  getSectionPos();
  getRandom();
  // console.log(sectionLiArr.value)
})
onUnmounted(() => {
  clearInterval(state.timer);
})
</script>

<style lang="scss">
* {
  font-family: 'Josefin Sans', sans-serif !important;
}

$bg_color: #fff;
$bg_sub_color: #134264;
$bg_secondary_color: #efefef;

$font_color: #0D2C43;
$font_sub_color: #6696B9;
$font_secondary_color: #4B82AB;
$font_sub_light: #c5c5c5;
$font_light: #fff;
$font_title: #F8DDAA;
$font_primary: #134264;

.header {
  // width: 100%;
  // height: 60px;
  // display: flex;
  // position: fixed;
  // left: 0;
  // top: 0;
  // justify-content: flex-end;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 999;

  // color: #fff;
  // ul{
  //   display: flex;
  //     justify-content: center;
  //     align-items: center;
  //     margin-right: 2rem;
  // }
  // li {
  //   margin: 0 1rem;
  // }
  .el-tabs {
    margin: 0 0.8rem;

    &__nav-wrap::after {
      background-color: transparent;
    }

    &__item {
      color: #fff;
      font-weight: 600;
      letter-spacing: 0.2rem;

      .is-active {
        color: $font_primary;
      }
    }

    &__content {
      display: none;
    }
  }
}

.content {

  // width: 100%;
  // .banner {
  // display: block;
  // background: url(./assets/images/banner.jpg);
  // width: 100%;
  // height: 1000px;
  // }

  .title {
    display: block;
    font-size: 2rem;
    font-weight: 700;
    text-align: center;
    // margin-top: 2rem;
    padding: 2rem;
    color: $font_secondary_color;
    // transform: translateY(50px);
    opacity: 0;
  }

  .intro {
    display: flex;
    justify-content: space-around;
    align-items: center;
    // padding: 1rem;
    // position: absolute;
    //   top: 55%;
    transform: translateY(-40rem);
    padding: 2rem;
    // left: calc( 50% - 18rem);
    // display: flex;
    // justify-content: center;
    background-color: rgba(0, 0, 0, 0.5);
    // margin-top: -16rem;
    // width: 36rem;
    // height: 20rem;
    border-radius: 10px;
    text-align: center;
    color: #fff;

    .headshot {
      background: #fff url(./assets/images/2017.11-1.jpg) no-repeat 15px 0/100%;
      width: 8rem;
      height: 8rem;
      border-radius: 50%;
      background-size: 80%;
    }

    .personal {
      text-align: left;
      font-size: 1.2rem;
      margin-left: 3rem;
      font-weight: 600;

      svg {
        width: 1.5rem;
        height: 1.5rem;
        color: #fff;
        margin: 0.5rem 0.5rem 0;
      }

      .line {
        display: flex;
        align-items: end;

        img {
          margin-left: 1rem;
          height: 26px;
        }
      }
    }

    @media screen and (max-width: 1280px) {
      transform: translateY(-30rem);
    }

    @media screen and (max-width: 1000px) {
      transform: translateY(-15rem);
    }

    @media screen and (max-width: 768px) {
      transform: translateY(-25rem);
    }

    @media screen and (max-width: 400px) {
      transform: translateY(-10rem);
    }
  }

  .skill-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 3rem;
    z-index: 1;

    &__box {
      display: flex;
      justify-content: center;
    }

    // box-shadow: 5px 5px 5px rgb(0 0 0 / 30%);
    &__ul {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      width: 92%;
      background-color: $font_primary;
      box-shadow: 5px 5px 5px rgb(0 0 0 / 30%);
      border-radius: 10px;
      border: none;
      padding: 1rem 0.5rem;

      .el-collapse-item {

        &__header {
          background-color: $font_primary;
          color: $font_title;
          font-size: 18px;
          letter-spacing: 0.1rem;
          font-weight: 600;
          padding-left: 1rem;
          // box-shadow: 5px 5px 5px rgb(0 0 0 / 30%);
          border-top-right-radius: 15px;
          border-top-left-radius: 15px;
          // border-radius: 15px;
          border: none;

          &:last-child {
            border-bottom-right-radius: 15px;
            border-bottom-left-radius: 15px;
          }
        }

        &__wrap {
          border: none;
          background-color: $font_secondary_color;
          border-bottom-right-radius: 10px;
          border-bottom-left-radius: 10px;
        }

        &__content {
          background-color: $font_secondary_color;
          color: $font_light;
          padding: 0.5rem;
          letter-spacing: 0.1rem;
        }
      }

      @media screen and (max-width: 768px) {
        margin: 1rem;
        padding: 1rem 0;

        .el-collapse-item {
          width: 100%;
        }
      }
    }

    // ul {
    //   display: flex;
    //   flex-wrap: wrap;
    //   justify-content: flex-start;
    //   width: 76%;

    //   li {
    //     background-color: #3C3C3C;
    //     font-size: 1.2rem;
    //     margin: 0.3rem 1rem;
    //     padding: 0.3rem 1rem;
    //     border-radius: 0.5rem;
    //     box-shadow: 5px 5px 5px rgb(0 0 0 / 30%);
    //     color: #fff;

    //     h3 {
    //       text-shadow: 5px 5px 3px#000;
    //       font-weight: 700;
    //     }

    //     span {
    //       font-size: 0.9rem;
    //     }
    //   }
    // }
  }

  .experience-content {
    background: url(./assets/images/25332.jpg) no-repeat 0 0/100%;
    background-position: center;
    background-attachment: fixed;
    position: relative;
    z-index: 0;
    padding-top: 2rem;

    &::before {
      content: "";
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      background-color: #fff;
      opacity: 0.8;
      z-index: -1;
    }

    .company-box {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

      .company-li {
        // background-color: #F4F8FB;
        // border: 1px solid #3C3C3C;
        margin-bottom: 2.8rem;
        padding: 1rem;
        background-color: #fff;
        border-radius: 1rem;
        box-shadow: 5px 5px 7px rgba(0, 0, 0, 0.5);

        h3 {
          font-size: 20px;
          font-weight: 600;
          color: $font_color;
          display: flex;

          img {
            width: 25px;
            height: 25px;
            margin-right: 0.2rem;
          }
        }

        h4 {
          font-weight: 600;

          span {
            color: $font_secondary_color;
          }
        }

        h5 {
          margin-top: 0.5rem;
          color: #FF8000;
        }

        .description {
          font-size: 14px;
          letter-spacing: 0.07rem;
        }

        .place {
          font-weight: 600;
        }

        .el-collapse {
          display: flex;
          align-items: center;
        }

        .el-collapse-item {
          &__header {
            margin-left: 0.8rem;
            color: #FF8000;
            font-weight: 600;
            font-size: 16px;
          }

          &__content {
            padding: 0.2rem 0 0 0.8rem;
          }
        }
      }
    }

    // @media screen and (max-width: 768px) {
    //   background: url(./assets/images/25332.jpg) no-repeat 0 0/100%;
    //   background-position: center;
    //   background-attachment: fixed;
    // }
  }

  .about-content {
    background-color: $font_color;
    padding: 2rem 5rem;
    position: relative;
    // .bg{
    //   background: url(@/assets/images/4458680.jpg) no-repeat 0 0/100%;
    //   position: absolute;
    //   z-index: 0;
    //   right: 0;
    // } 
    p {
      color: $font_title;
      font-size: 14px;
      text-align: left;
      letter-spacing: 0.1rem;
      line-height: 1.5rem;
    }
  }

  .portfolio-content {
    .portfolio-box {
      padding: 2rem;
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      font-size: 14px;

      .card {
        padding: 0.5rem;
      }

      >li {
        margin: 1rem 2rem;
        border: 1px solid $font_color;
        box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.5);
        position: relative;
        letter-spacing: 0.1rem;
        &:hover {
          // cursor: default;
          background-color: rgba(248, 221, 170, 0.3);
        }

        img {
          width: 100%;
          height: auto;

          &:hover {
            cursor: pointer;
          }
        }

        .sub-title {
          font-size: 16px;
          padding: 0.5rem;
          text-align: left;
          color: $font_secondary_color;
          font-weight: 700;
        }

        .description {
          padding: 0.5rem;
          line-height: 1rem;
        }

        .info {
          padding: 0 0.5rem;
          color: $font_primary;

          >li {
            display: flex;

            p {
              margin-left: 0.2rem;
            }
          }
        }
        .text {
          padding: 0.5rem;
        }
      }
    }
  }

  .skill {
    display: flex;
    flex-wrap: wrap;

    p {
      margin: 0.2rem;
      padding: 0.1rem 0.5rem;
      border-radius: 15px;
      background-color: $font_primary;
      color: $font_light;
      letter-spacing: 0.08rem;
      box-shadow: 2px 2px 2px rgba(0, 0, 0, 0.3);
    }
  }

  .fade-in {
    opacity: 1;
    transition: all 1s;
    animation: show 1s;
  }

  .fade-out {
    opacity: 0;
    transition: all 1s;
    animation: hide 1s;
  }

  .jitter {
    transition: all 0.5s;
    animation: bounce 1s;
  }
}

.footer {
  background-color: $font_color;

  p {
    color: $font_light;
    font-size: 14px;
    text-align: center;
    letter-spacing: 0.1rem;
    line-height: 1.5rem;
    padding: 2rem;
  }
}

@media screen and (max-width: 768px) {
  .content {
    .intro {
      flex-direction: column;

      .personal {
        margin-left: 1rem;
      }
    }

    .skill-content {
      padding: 2rem 0;

      ul {
        width: 100%;
        align-content: center;
        justify-content: center;
      }
    }
    .about-content {
      padding: 2rem;
    }
  }
}

@keyframes show {
  0% {
    opacity: 0;
    transform: translateY(50px);
  }

  100% {
    opacity: 1;
    transform: translateY(0px);
  }
}

@keyframes hide {
  0% {
    opacity: 1;
    transform: translateY(0px);
  }

  100% {
    opacity: 0;
    transform: translateY(50px);
  }
}

@keyframes bounce {
  0% {
    transform: rotate(5deg);
  }

  30% {
    transform: rotate(-5deg);
  }

  75% {
    transform: rotate(3deg);
  }

  100% {
    transform: rotate(0deg);
  }
}
</style>
