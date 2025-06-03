<script setup lang="ts">
import './main.css'
import { computed, reactive, ref } from 'vue'

//定义常量

//刘文博图片
const lwb = [
  { img: '/lwb/cap/1.png', id: 1 },
  { img: '/lwb/cap/2.png', id: 2 },
  { img: '/lwb/cap/3.png', id: 3 },
  { img: '/lwb/cap/4.png', id: 4 },
  { img: '/lwb/cap/5.png', id: 5 },
  { img: '/lwb/cap/6.png', id: 6 },
  { img: '/lwb/cap/7.png', id: 7 },
  { img: '/lwb/cap/8.png', id: 8 },
  { img: '/lwb/cap/9.png', id: 9 },
]

//瓦的图片
const vals = [
  { id: '21gj/champions 2021' },
  { id: '23gj/champions2023' },
  { id: '23gj/champions2023' },
  { id: "gy/gaia's vengeance" },
  { id: 'lh/mystbloom' },
  { id: 'ln/primordium' },
  {
    id: 'long/imperium',
  },
  { id: 'nz/neofrontier' },
  { id: 'sg/neptune' },
  { id: 'tj/sovereign, ep 8' },
  { id: 'vct/vct' },
]
//定义变量
const disp = ref({ lwb: true, startpage: true, cap: false, sel: false, ac: false, capon: false, capoff: false, over: false, stat: false })
const hards = ref({ tip: '选择一个难度吧~', sc: '尽力而为吧！' })
const seting = ref({ hard: 1, sc: 0 })
const ingame = ref({ sc: 0, cl: 0, lx: 0, lxn: 0, scs: 0, time: 30 })
const ac = ref({ 1: '', 2: '' })
const randomlwb = ref()
const capval = ref({ num: 0 })
const aim = reactive<Point[]>([])
interface Point {
  x: string;
  y: string;
  id: number;
}
const val = ref({
  id: 0, img: ''
})
const overs = ref({ sc: 0, as: 0, hh: '' })

const d = new Audio('/die.mp3')
d.volume = 0.3

//刘文博呼吸灯

function startpagelwb() {
  if (disp.value.lwb) {
    const lwb = document.getElementById('lwb')
    lwb!.style.height = 20 + 'vh'
    setTimeout(() => {
      const lwb = document.getElementById('lwb')
      lwb!.style.height = 10 + 'vh'
      setTimeout(() => {
        startpagelwb()
      }, 1000);
    }, 1000);

  } else {
    console.log('stopd')
  }
}

//dom加载完才可以开始循环，不然无法选中


//点击开始刘文博
function startpagehit() {
  disp.value.lwb = false
  const startpage = document.getElementById('startpage')
  startpage!.style.opacity = '0'
  setTimeout(() => {
    disp.value.startpage = false
    disp.value.sel = true
  }, 500);

}

//难度提示
function hardtips(e: number) {
  if (e === 0) {
    hards.value.tip = '🦌的不会瞄准瞎几把点就行'
    hards.value.sc = '小心你电脑 | 0x得分效率 '
  }
  if (e === 1) {
    hards.value.tip = '非常简单！即使是连🦌100天的杂鱼也可以轻易过关哦'
    hards.value.sc = '马枪不会扣分 | 0.5x得分效率 '
  }
  if (e === 2) {
    hards.value.tip = '有点困难！即使是连🦌50天以下的高手才能通关'
    hards.value.sc = '马枪会扣分 | 1x得分效率'
  }
  if (e === 3) {
    hards.value.tip = '请输入文本'
    hards.value.sc = '马枪会扣分 | 2x得分效率 '
  }
}

//检测开始时是否含有cookie
function preconfig(val: number) {
  seting.value.hard = val
  disp.value.sel = false
  if (val === 0) {
    seting.value.sc = 0
  }
  if (val === 1) {
    seting.value.sc = 0.5
  }
  if (val === 2) {
    seting.value.sc = 1
  }
  if (val === 3) {
    seting.value.sc = 2
  }
  if (document.cookie) {
    startgame()
  } else {
    console.log('用户未通过人机验证')
    disp.value.sel = false
    disp.value.ac = true
    setTimeout(() => {
      ac.value[1] = 'wait,我们要验证你是人类吗'

    }, 500);
    setTimeout(() => {
      ac.value[2] = '别担心，这很容易'
    }, 2000);
    setTimeout(() => {
      disp.value.ac = false
      disp.value.cap = true
      cap()
    }, 3000);
  }
}

//开始游戏逻辑
function startgame() {

  //设置
  selval()
  ingame.value.time = 30
  ingame.value.sc = 0
  setTimeout(() => {
    ingame.value.cl = 0
  }, 0);
  const mod = seting.value.hard
  disp.value.over = false
  console.log(mod)
  //模式设定
  if (mod === 0) {
    disp.value.stat = true
    const timer = setInterval(() => {
      createlwb()
      createlwb()
      createlwb()
      createlwb()
      createlwb()
      createlwb()

    }, 1);
    const timer2 = setInterval(() => {
      ingame.value.time--
    }, 1000)
    setTimeout(() => {
      clearInterval(timer)
      clearInterval(timer2)
      window.location.href = "/"
    }, 30000);
  }
  if (mod === 1) {
    createlwb()
    createlwb()
    createlwb()
    disp.value.stat = true
    const timer = setInterval(() => {
      ingame.value.time--
    }, 1000)
    setTimeout(() => {
      clearInterval(timer)
      over()
    }, 30000);
  }
  if (mod === 2) {
    createlwb()
    createlwb()
    createlwb()
    disp.value.stat = true

    const timer = setInterval(() => {
      ingame.value.time--
    }, 1000)
    const timer2 = setInterval(() => {
      aim.splice(0, 1)
      createlwb()
    }, 2000);
    setTimeout(() => {
      clearInterval(timer)
      clearInterval(timer2)
      over()
    }, 30000);
  }
  if (mod === 3) {
    console.log('3')
    createlwb()
    createlwb()
    createlwb()
    createlwb()
    createlwb()
    createlwb()
    disp.value.stat = true
    const timer = setInterval(() => {
      ingame.value.time--
    }, 1000)
    const timer2 = setInterval(() => {
      const a = (Math.floor(Math.random() * 4) + 1)
      aim.splice(0, a)
      for (let i = 0; i < a; i++) {
        createlwb()
      }
    }, 200);
    setTimeout(() => {
      clearInterval(timer)
      clearInterval(timer2)
      over()
    }, 30000);
  }
}


//击中逻辑
function stin(e: Event) {
  d.play()
  //动画逻辑
  document.getElementById("statimg")!.style.height = '0'
  document.getElementById('statimg')!.style.transform = 'rotate(0deg)'
  setTimeout(() => {
    document.getElementById('statimg')!.style.transform = 'rotate(' + 720 * ingame.value.lxn + 'deg)'
    document.getElementById("statimg")!.style.height = '100%'
  }, 0);
  //生成逻辑
  const id: number = JSON.parse((e.target as HTMLInputElement).id)
  aim.splice(id, 1)
  createlwb()
}
function createlwb() {

  const val = document.getElementById('main')!.getBoundingClientRect()
  const w = val.width
  const h = val.height
  const l = Math.floor(Math.random() * (w - w / 20))
  const t = Math.floor(Math.random() * (h - h / 5))

  if (l + w / 20 > w && t + h / 5 > h) {
    createlwb()
  } else {
    const ll = l + 'px'
    const tt = t + 'px'

    aim.push({ x: ll, y: tt, id: Math.floor(Math.random() * 13) })
  }
}

//分数逻辑
const realsc = computed({
  get: () => Math.floor((ingame.value.sc * (ingame.value.sc / ingame.value.cl)) * seting.value.sc + (ingame.value.lxn * seting.value.sc)),
  set: () => { }
})
//人机验证
function cap() {
  capval.value.num = 0
  const shuffleArray = (array: Array<{ id: number, img: string }>) => {
    const newArray = [...array]
    for (let i = newArray.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1))
        ;[newArray[i], newArray[j]] = [newArray[j], newArray[i]]
    }
    return newArray
  }
  console.log(shuffleArray(lwb))
  randomlwb.value = shuffleArray(lwb)
}

//人机验证处理逻辑
function selover() {
  if (capval.value.num === 0) {
    document.cookie = 'cap=1'

    document.getElementById('imgs')!.classList.add('op')
    disp.value.capon = true
    setTimeout(() => {
      disp.value.cap = false
      document.getElementById('imgs')!.classList.remove('op')
      startgame()
    }, 2000);
  } else {
    document.getElementById('imgs')!.classList.add('op')
    disp.value.capoff = true
    setTimeout(() => {
      document.getElementById('imgs')!.classList.remove('op')
      disp.value.capoff = false
      disp.value.cap = false
      setTimeout(() => {
        disp.value.cap = true
      }, 0);
      cap()
    }, 2000);
  }
}

//人机验证选择逻辑
function seled(e: Event) {
  const id = (e.target as HTMLElement).id
  if (document.getElementById(id)?.className === 'seled') {
    document.getElementById(id)?.classList.remove('seled')
    capval.value.num--
  } else {
    document.getElementById(id)?.classList.add('seled')
    capval.value.num++
  }
}
//点击事件监听
function hdclick(e: Event) {
  try {
    const a = JSON.parse((e.target as HTMLElement).id)
    if (typeof (a) === 'number') {
      if (ingame.value.cl - ingame.value.sc === ingame.value.lx) {
        ingame.value.lxn++
        valorant()
      } else {
        ingame.value.lxn = 1
        ingame.value.lx = ingame.value.cl - ingame.value.sc
        valorant()
      }
      ingame.value.sc++
      ingame.value.cl++
    }
  } catch {
    ingame.value.cl++
  }

  console.log(ingame.value.cl, ingame.value.sc, ingame.value.lx, ingame.value.lxn)
}
//下方图片
function valorant() {
  const id = Number(val.value.id)
  const lwb = new Audio('/gethet.mp3')
  if (ingame.value.lxn <= 5) {
    const ad = new Audio('/val/' + vals[id].id + '_' + ingame.value.lxn + '_kills_audio.mp3')
    val.value.img = '/val/' + vals[id].id + '_' + ingame.value.lxn + '_kills.png'
    ad.volume = 0.7
    ad.play()
  } else {
    const ad = new Audio('/val/' + vals[id].id + '_' + 6 + '_kills_audio.mp3')
    val.value.img = '/val/' + vals[id].id + '_' + 6 + '_kills.png'
    ad.volume = 0.7
    ad.play()
  }
  lwb.play()
  lwb.volume = 1
  console.log(val.value.img)
}
function alertd(msg: string) {
  alert(msg)
}
function selval() {
  val.value.id = Math.floor(Math.random() * 10)
}
function over() {
  overs.value.sc = Math.floor((ingame.value.sc * (ingame.value.sc / ingame.value.cl)) * seting.value.sc + (ingame.value.lxn * seting.value.sc))
  overs.value.as = Math.floor((ingame.value.sc / ingame.value.cl) * 100)
  overs.value.hh = localStorage.getItem('top') || '你tm都没玩过'
  if (overs.value.sc > Number(localStorage.getItem('top')) || !localStorage.getItem('top')) {
    localStorage.setItem('top', JSON.stringify(overs.value.sc))
  }
  disp.value.stat = false
  disp.value.over = true
  aim.splice(0, aim.length)
}
</script>

<template>
  <div class="main" @click="hdclick($event)" id="main">
    <img v-for="(a, index) in aim" :key="index" :src="'/lwb/' + a.id + '.png'" class="aim"
      :style="{ 'top': a.y, 'left': a.x }" @click="stin($event)" :id="JSON.stringify(index)">
    <div class="over" v-if="disp.over">
      <h1>游戏结束</h1>
      <span>得分：{{ overs.sc }}</span><span>准确率：{{ overs.as }}%</span>
      <span>历史最高：{{ overs.hh }}</span>
      <div class="but"><button @click="startgame()">再来</button><button onclick="window.location.href='/'">返回主页</button>
      </div>

      <div class="cg" v-if="overs.sc <= 20">
        <h2>你是人类吗？</h2>
        <img src="/lwb/cg/ls2.png" alt="">
        <p>菜比，门外竖着！</p>
      </div>
      <div class="cg" v-if="overs.sc > 20 && overs.sc <= 50">
        <h2>好好学吧小子！</h2>
        <img src="/lwb/cg/pro.png" alt="">
        <p>会变强的！</p>
      </div>
      <div class="cg" v-if="overs.sc >= 50">
        <h2>主人</h2>
        <img src="/lwb/cg/win1.jpg" alt="">
        <p>请输入文本</p>
      </div>
    </div>


    <div id="startpage" style="transition: 0.5s;" v-if="disp.startpage">
      <div class="title" id="title">
        <h1>拷打刘文博</h1>
        <h2>全射给刘文博!</h2>
      </div>
      <div class="capt">
      </div>
      <div class="start">
        <div class="lwb" id="lwb">
          <img src="/lwb/lwb1.png" @click="startpagehit()" @load="startpagelwb()" alt="">
          <p>拷打刘文博以开始游戏</p>
        </div>
      </div>
    </div>
    <div id="sel" v-if="disp.sel">
      <h1>选择难度</h1>
      <div class="hards">
        <span @mouseenter="hardtips(0)" @click="preconfig(0)">杏无能</span>
        <span @mouseenter="hardtips(1)" @click="preconfig(1)"> 杂鱼 </span>
        <span @mouseenter=" hardtips(2)" @click="preconfig(2)">楚男</span>
        <span @mouseenter="hardtips(3)" @click="preconfig(3)">打柱机</span>
      </div>
      <p>{{ hards.tip }}</p>
      <p>{{ hards.sc }}</p>
    </div>
    <div class="ac" v-if="disp.ac">
      <h1>{{ ac[1] }}</h1>
      <h1>{{ ac[2] }}</h1>
    </div>
    <div class="cap" v-if="disp.cap">
      <h2 style="color: #ff2333;">请先进行人机验证</h2>
      <p>请选出下图中的人类</p>
      <div class="box">
        <div class="imgs" id="imgs"><img v-for="(w, index) in randomlwb" :key='index' :src="w.img" alt=""
            :id="String(index)" @click="seled($event)"></div>
        <h1 v-if="disp.capoff" class="h12">你个人机</h1>
        <h1 v-if="disp.capon" class="h11">游戏即将开始</h1>
      </div>
      <div class="down"><a>@lwb人机验证</a>|<a @click="alertd('你有几把隐私')">隐私声明</a><button class="selover"
          @click="selover()">选完了</button>
      </div>
    </div>
    <div class="stat" v-if="disp.stat">
      <span>时间：{{ ingame.time }}</span><img :src="val.img" id="statimg" alt=""><span v-if="seting.hard !== 0">分数：{{
        realsc }}</span>
      <span v-if="seting.hard === 0">刘文博数量：{{ aim.length }}</span>
    </div>
  </div>
  <div class="clove">

  </div>
</template>
