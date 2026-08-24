<template>
  <div ref="containerRef" class="spectrum-container">
    <canvas ref="canvasRef" class="spectrum"></canvas>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, nextTick } from 'vue'
import space from 'color-space'

const LMS = [
[ 390,  -3.38195,  -3.43374,  -2.02012 ],
[ 395,  -2.97802,  -3.01834,  -1.62297 ],
[ 400,  -2.61828,  -2.64399,  -1.24680 ],
[ 405,  -2.31575,  -2.32789,  -0.91204 ],
[ 410,  -2.05942,  -2.05583,  -0.63263 ],
[ 415,  -1.87342,  -1.83780,  -0.41866 ],
[ 420,  -1.73405,  -1.66424,  -0.26471 ],
[ 425,  -1.63956,  -1.52913,  -0.17103 ],
[ 430,  -1.54994,  -1.40388,  -0.09553 ],
[ 435,  -1.46718,  -1.28550,  -0.04404 ],
[ 440,  -1.39517,  -1.18857,  -0.00392 ],
[ 445,  -1.34739,  -1.11987,  -0.00370 ],
[ 450,  -1.30221,  -1.06022,  -0.01982 ],
[ 455,  -1.25695,  -1.00792,  -0.06538 ],
[ 460,  -1.18899,  -0.93452,  -0.10419 ],
[ 465,  -1.09318,  -0.84001,  -0.13179 ],
[ 470,  -1.00228,  -0.75475,  -0.18953 ],
[ 475,  -0.92518,  -0.68740,  -0.28700 ],
[ 480,  -0.85342,  -0.62754,  -0.40856 ],
[ 485,  -0.78528,  -0.57176,  -0.53712 ],
[ 490,  -0.71770,  -0.51766,  -0.67394 ],
[ 495,  -0.63278,  -0.44726,  -0.79446 ],
[ 500,  -0.53916,  -0.36880,  -0.91066 ],
[ 505,  -0.44404,  -0.28770,  -1.05112 ],
[ 510,  -0.35293,  -0.21076,  -1.21595 ],
[ 515,  -0.27044,  -0.14318,  -1.36843 ],
[ 520,  -0.20165,  -0.08799,  -1.53457 ],
[ 525,  -0.15198,  -0.05279,  -1.71240 ],
[ 530,  -0.11315,  -0.02887,  -1.89959 ],
[ 535,  -0.08317,  -0.01374,  -2.09181 ],
[ 540,  -0.05502,  -0.00208,  -2.29337 ],
[ 545,  -0.03665,  -0.00122,  -2.49909 ],
[ 550,  -0.02678,  -0.01002,  -2.70798 ],
[ 555,  -0.01514,  -0.01928,  -2.91982 ],
[ 560,  -0.00813,  -0.03728,  -3.13067 ],
[ 565,  -0.00240,  -0.05888,  -3.34105 ],
[ 570,   0.00000,  -0.08964,  -3.55006 ],
[ 575,  -0.00335,  -0.13060,  -3.75687 ],
[ 580,  -0.01348,  -0.18490,  -3.96077 ],
[ 585,  -0.01972,  -0.24215,  -4.16116 ],
[ 590,  -0.03261,  -0.30751,  -4.35751 ],
[ 595,  -0.05258,  -0.38590,  -4.54940 ],
[ 600,  -0.07884,  -0.47570,  -4.73646 ],
[ 605,  -0.11064,  -0.57696,  -4.91841 ],
[ 610,  -0.15137,  -0.68767,  -5.09502 ],
[ 615,  -0.20013,  -0.80620,  -5.26613 ],
[ 620,  -0.25631,  -0.93315,  -99.0000 ],
[ 625,  -0.31881,  -1.06759,  -99.0000 ],
[ 630,  -0.39717,  -1.20682,  -99.0000 ],
[ 635,  -0.48431,  -1.35176,  -99.0000 ],
[ 640,  -0.57547,  -1.50268,  -99.0000 ],
[ 645,  -0.67104,  -1.66147,  -99.0000 ],
[ 650,  -0.78215,  -1.81113,  -99.0000 ],
[ 655,  -0.90396,  -1.97013,  -99.0000 ],
[ 660,  -1.03148,  -2.13653,  -99.0000 ],
[ 665,  -1.16425,  -2.30349,  -99.0000 ],
[ 670,  -1.30219,  -2.46386,  -99.0000 ],
[ 675,  -1.44584,  -2.62412,  -99.0000 ],
[ 680,  -1.59552,  -2.78586,  -99.0000 ],
[ 685,  -1.75153,  -2.95029,  -99.0000 ],
[ 690,  -1.91471,  -3.11859,  -99.0000 ],
[ 695,  -2.07203,  -3.27946,  -99.0000 ],
[ 700,  -2.22933,  -3.43733,  -99.0000 ],
[ 705,  -2.38814,  -3.59616,  -99.0000 ],
[ 710,  -2.55215,  -3.75845,  -99.0000 ],
[ 715,  -2.71657,  -3.91862,  -99.0000 ],
[ 720,  -2.87717,  -4.07483,  -99.0000 ],
[ 725,  -3.03726,  -4.22963,  -99.0000 ],
[ 730,  -3.19425,  -4.38086,  -99.0000 ],
[ 735,  -3.35063,  -4.53113,  -99.0000 ],
[ 740,  -3.50742,  -4.68015,  -99.0000 ],
[ 745,  -3.65890,  -4.82259,  -99.0000 ],
[ 750,  -3.81093,  -4.96577,  -99.0000 ],
[ 755,  -3.96056,  -5.10664,  -99.0000 ],
[ 760,  -4.10795,  -5.24482,  -99.0000 ],
[ 765,  -4.25472,  -5.38300,  -99.0000 ],
[ 770,  -4.39871,  -5.51901,  -99.0000 ],
[ 775,  -4.54339,  -5.65541,  -99.0000 ],
[ 780,  -4.68336,  -5.78666,  -99.0000 ],
[ 785,  -4.82266,  -5.91702,  -99.0000 ],
[ 790,  -4.96080,  -6.04616,  -99.0000 ],
[ 795,  -5.09813,  -6.17419,  -99.0000 ],
[ 800,  -5.23280,  -6.29827,  -99.0000 ],
[ 805,  -5.36542,  -6.00000,  -99.0000 ],
]

function lms(nm: number) : [number, number, number]
{
  if (nm < 390) return [0,0,0]
  if (nm > 804) return [0,0,0]
  let idx = Math.round((nm-390)/5)  
  let val0 = LMS[idx]
  let val1 = LMS[idx+1]
  if (!val0) return [0,0,0]
  if (!val1) val1=val0

  val0 = [ Math.exp(val0[1]), Math.exp(val0[2]), Math.exp(val0[3])]
  val1 = [ Math.exp(val1[1]), Math.exp(val1[2]), Math.exp(val1[3])]
  
  let f = (nm-((idx*5)+390)) / 5.0
  return [
    val0[0] + f*(val1[0]-val0[0]),
    val0[1] + f*(val1[1]-val0[1]),
    val0[2] + f*(val1[2]-val0[2]),
  ]
}


interface Props {
  low?: number
  high?: number
  byFreq?: boolean
}

const props = withDefaults(defineProps<Props>(), {
  low: 380,
  high: 750,
  byFreq: false
})

const C : number = 299792458.0

const containerRef = ref(null)
const canvasRef = ref(null)
let ctx: CanvasRenderingContext2D | null = null
let colorFunc: any = null
let colorSpace: string = "srgb"

// Resize function
function resizeCanvas() {
  if (!containerRef.value || !canvasRef.value) return

  const container : HTMLDivElement = containerRef.value
  const canvas : HTMLCanvasElement = canvasRef.value

  // Handle high-DPI screens
  const dpr = window.devicePixelRatio || 1
  canvas.width = container.clientWidth * dpr
  canvas.height = container.clientHeight * dpr
  canvas.style.width = container.clientWidth + 'px'
  canvas.style.height = container.clientHeight + 'px'

  if (!ctx)
  {
    try
    {
      ctx = canvas.getContext('2d', { colorSpace: 'rec2100-pq', pixelformat: 'float16' }) as CanvasRenderingContext2D
      // rec2100 color space is not supported by CSS colors yet, soo we can't draw in it directly.
      colorFunc = space.wavelength.rec2020
      colorSpace = 'rec2020'
      console.log('Using HDR float canvas with rec2020 color space')
    }
    catch (e)
    {
      ctx = null
    }
  }
  if (!ctx)
  {
    try
    {
      ctx = canvas.getContext('2d', { colorSpace: 'rec2100-pq' }) as CanvasRenderingContext2D
      // rec2100 color space is not supported by CSS colors yet, soo we can't draw in it directly.
      colorFunc = space.wavelength.rec2020
      colorSpace = 'rec2020'
      console.log('Using HDR canvas with rec2020 color space')
    }
    catch (e)
    {
      ctx = null
    }
  }
  if (!ctx)
  {
    try
    {
      ctx = canvas.getContext('2d', { colorSpace: 'display-p3' })
      colorFunc = space.wavelength.p3
      colorSpace = 'display-p3'
      console.log('Using SDR canvas with display-p3 color space')
    }
    catch (e)
    {
      ctx = null
    }
  }
  if (!ctx)
  {
    try
    {
      ctx = canvas.getContext('2d', { colorSpace: 'srgb' })
      colorFunc = space.wavelength.rgb
      colorSpace = 'srgb'
      console.log('Using SDR canvas with srgb color space')
    }
    catch (e)
    {
      ctx = null
    }
  }
  if (!ctx) return

  ctx.setTransform(1, 0, 0, 1, 0, 0) // Reset transform before scaling
  ctx.scale(dpr, dpr)

  draw()
}

function roundTo(value: number, multiple: number, floor: boolean): number
{
  if (Number.isNaN(value) || Number.isNaN(multiple) || multiple === 0) {
    return value; // Return the original value if invalid
  }

  return floor ? Math.floor(value / multiple) * multiple : Math.ceil(value / multiple) * multiple;
}

function toCanvas(x: number): number {
  return ((x - props.low) / (props.high - props.low)) * ctx!.canvas.width
}

function nmToThz(nm: number): number {
  return (C / (nm * 1e-9)) / 1e12
}

function thzToNm(thz: number): number {
  return (C / (thz * 1e12)) / 1e-9
}

function fromCanvas(x: number): number {
  return (x / ctx!.canvas.width) * (props.high - props.low) + props.low
}

function scale125(val: number): number
{
  let sign = 1
  if (val < 0)
  {
    sign = -1
    val = -val
  }
  let exp=0
  while (val > 10.0)
  {
    val /= 10.0
    exp += 1
  }
  while (val < 1.0)
  {
    val *= 10.0
    exp -= 1
  }
  if (val > 5.0)
  {
    val = 5.0
  }
  else if (val > 2.0)
  {
    val = 2.0
  }
  else
  {
    val = 1.0
  }
  return val * Math.pow(10, exp) * sign
}

// Example drawing
function draw() {
  if (!ctx) return
  const { width, height } = { width: ctx.canvas.width, height: ctx.canvas.height }

  ctx.fillStyle = 'black'
  ctx.fillRect(0, 0, width, height)

  let gradTop = 0
  let gradBottom = height
  if (height > 100)
  {
    gradTop=20
    gradBottom=height-20

    // draw a grid
    ctx.strokeStyle = 'gray'
    ctx.fillStyle = 'gray'
    ctx.textAlign = 'center'
    ctx.textBaseline = 'top'

    ctx.beginPath()
    ctx.moveTo(0, gradTop-1)
    ctx.lineTo(width, gradTop-1)
    ctx.moveTo(0, gradBottom+1)
    ctx.lineTo(width, gradBottom+1)
    ctx.stroke()

    const step = scale125((props.high-props.low)/5.0)
    let leftMarker = roundTo(props.low, step, true)
    let rightMarker = roundTo(props.high, step, false)

    for (let x = leftMarker; (rightMarker - x) / step >= 0; x += step)
    {
      const xPos = toCanvas(x)
      if (xPos < 0 || xPos > width) continue

      let topText = ''
      let bottomText = ''
      if (props.byFreq)
      {
        topText = x.toFixed(2) + ' THz'
        bottomText = thzToNm(x).toFixed(2) + ' nm'
      } else {
        topText = nmToThz(x).toFixed(2) + ' Thz'
        bottomText = x.toFixed(2) + ' nm'
      }

      ctx.fillText(topText, xPos, 0)

      ctx.beginPath()
      ctx.moveTo(xPos, 10)
      ctx.lineTo(xPos, gradTop-1)
      ctx.moveTo(xPos, gradBottom+1)  
      ctx.lineTo(xPos, height-10)
      ctx.stroke()

      ctx.fillText(bottomText, xPos, height - 10)
    }
  }

  // if we havce room, draw the response curves of human cone cells on top
  if (height > 200)
  {
    ctx.fillStyle = 'gray'
    ctx.textAlign = 'left'
    ctx.textBaseline = 'top'
    ctx.fillText(`colorspace: ${colorSpace}`, 0, gradTop+2)


    const scale = (height/2 - gradTop)
    gradTop = height/2

    let values = []
    for (let x = 0; x < width; ++x)
    {
      const nm = props.byFreq ? thzToNm(fromCanvas(x)) : fromCanvas(x)
      values.push(lms(nm))
    }
    const strokes = [ 'red', 'green', 'blue' ]
    for (let c = 0; c<3; ++c)
    {
      ctx.strokeStyle = strokes[c]
      ctx.beginPath()
      ctx.moveTo(0, gradTop - 1 - values[0][c]*scale)
      for (let x = 1; x < width; ++x)
      {
        ctx.lineTo(x, gradTop - 1 - values[x][c]*scale)
      }
      ctx.stroke();
    }

    // draw a divider
    ctx.strokeStyle = 'gray'
    ctx.beginPath()
    ctx.moveTo(0, gradTop-1)
    ctx.lineTo(width, gradTop-1)
    ctx.stroke()
  }

  for (let x = 0; x < width; ++x)
  {
    const nm = props.byFreq ? thzToNm(fromCanvas(x)) : fromCanvas(x)
    const [r,g,b] = colorFunc ? colorFunc(nm) : [0, 0, 0]
    ctx.fillStyle = `color(${colorSpace} ${r} ${g} ${b})`
    ctx.fillRect(x, gradTop, 1, gradBottom-gradTop)
  }
}

onMounted(async () => {
  await nextTick()
  resizeCanvas()
  window.addEventListener('resize', resizeCanvas)
})

onBeforeUnmount(() => {
  window.removeEventListener('resize', resizeCanvas)
})
</script>

<style scoped>
.spectrum-container {
  height: 400px;
  display: block;
  margin: 80px;
}
</style>
