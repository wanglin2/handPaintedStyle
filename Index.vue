<template>
  <div class="container" ref="container">
    <div>
      <canvas class="canvas" ref="canvas" width="500" height="500"></canvas>
      <canvas class="canvas" ref="canvas2" width="500" height="500"></canvas>
    </div>
    <div class="btn">
      <el-button type="primary" @click="toggle">{{
        isRefresh ? '停止' : '自动刷新'
      }}</el-button>
      <el-button type="primary" @click="handleRefresh">手动刷新</el-button>
    </div>
  </div>
</template>

<script>
import rough from 'roughjs/bundled/rough.esm.js'

export default {
  name: 'Index',
  data () {
    return {
      rc: null,
      ctx: null,
      offset: 5,
      isRefresh: false,
      timer: null
    }
  },
  mounted () {
    this.demo()
    this.ctx = this.$refs.canvas2.getContext('2d')
    this.draw()
  },
  methods: {
    /**
     * @Author: 王林25
     * @Date: 2021-02-05 10:11:39
     * @Desc: 示例
     */
    demo () {
      let ctx = this.$refs.canvas.getContext('2d')
      ctx.clearRect(0, 0, 500, 500)
      this.rc = rough.canvas(this.$refs.canvas)
      // this.rc.rectangle(100, 150, 300, 200, {
      //   roughness: 3,
      //   hachureGap: 15,
      //   fill: 'red',
      //   fillStyle: ''
      // })
      // this.rc.circle(195, 220, 40, {
      //   // fill: 'red'
      //   disableMultiStroke: true
      // })
      // this.rc.circle(325, 220, 40, {
      //   // fill: 'red'
      //   disableMultiStroke: true
      // })
      // this.rc.circle(195, 220, 200, {
      //   disableMultiStroke: true
      // })
      // this.rc.rectangle(225, 270, 80, 30, {
      //   fill: 'red',
      //   fillweight: 5
      // })
      // this.rc.line(200, 150, 150, 80, { roughness: 5 })
      // this.rc.line(300, 150, 350, 80, { roughness: 2 })
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-18 19:55:29
     * @Desc: 手动刷新
     */
    handleRefresh () {
      this.draw()
      this.demo()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-05 10:13:13
     * @Desc: 切换
     */
    toggle () {
      if (this.isRefresh) {
        clearTimeout(this.timer)
        this.isRefresh = false
      } else {
        this.isRefresh = true
        this.refresh()
      }
    },
    /**
     * @Author: 王林25
     * @Date: 2021-02-05 10:11:45
     * @Desc: 刷新
     */
    refresh () {
      if (!this.isRefresh) {
        return
      }
      this.timer = setTimeout(() => {
        this.demo()
        this.draw()
        this.refresh()
      }, 500)
    },
    /**
     * @Author: 王林25
     * @Date: 2021-02-05 10:12:22
     * @Desc: 绘制
     */
    draw () {
      this.ctx.clearRect(0, 0, 500, 500)
      // 线段
      // this.line(200, 150, 150, 80)
      // this.line(300, 150, 350, 80)
      // this.line(400, 150, 400, 350)

      // 矩形
      this.rectangle(100, 150, 300, 200, {
        fillType: 'line',
        fillStyle: '#ff9587',
        fillLineWidth: 4,
        gap: 10,
        doubleLine: false,
        tailLink: false,
        rotate: Math.floor(Math.random() * 70 + 10) * (Math.random() > 0.5 ? 1 : -1)
      })

      // 随机多边形
      // let p = []
      // let num = Math.ceil(Math.random() * 5) + 2
      // for (let i = 0; i < num; i++) {
      //   p.push([Math.floor(Math.random() * 250), Math.floor(Math.random() * 250)])
      // }
      // this.polygon(p, {
      //   fillType: 'line',
      //   fillStyle: '#ff9587',
      //   fillLineWidth: 4,
      //   gap: 10,
      //   doubleLine: false,
      //   tailLink: true
      // })

      // 正圆
      // this.ctx.beginPath()
      // this.ctx.strokeStyle = '#ccc'
      // this.ctx.arc(195, 220, 100, 0, 2 * Math.PI)
      // this.ctx.stroke()

      // 圆
      // this.circle(250, 250, 100, {
      //   fillType: 'line',
      //   fillStyle: '#ff9587',
      //   fillLineWidth: 4,
      //   gap: 10,
      //   doubleLine: false,
      //   tailLink: false,
      //   rotate:
      //     Math.floor(Math.random() * 70 + 10) * (Math.random() > 0.5 ? 1 : -1)
      // })
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-04 15:59:52
     * @Desc: 绘制线段
     */
    line (x1, y1, x2, y2) {
      this.drawDoubleLine(x1, y1, x2, y2)
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-05 11:11:52
     * @Desc: 绘制两条线段
     */
    drawDoubleLine (x1, y1, x2, y2, opt) {
      // 绘制原线段
      // this.ctx.save()
      // this.ctx.beginPath()
      // this.ctx.moveTo(x1, y1)
      // this.ctx.lineTo(x2, y2)
      // this.ctx.strokeStyle = '#999'
      // this.ctx.stroke()
      // this.ctx.restore()

      let line1 = this._line(x1, y1, x2, y2, opt)
      let line2 = this._line(x1, y1, x2, y2, opt)
      // this.drawPoint(line1[4], line1[5])
      // this.drawPoint(line1[6], line1[7])
      // this.drawPoint(line2[4], line2[5])
      // this.drawPoint(line2[6], line2[7])
      this.drawLine(line1, opt)
      this.drawLine(line2, opt)
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 20:15:06
     * @Desc: 绘制一条线段
     */
    drawSingleLine (x1, y1, x2, y2, opt) {
      let line = this._line(x1, y1, x2, y2, opt)
      this.drawLine(line, opt)
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-04 16:41:04
     * @Desc: 生成曲线
     */
    _line (x1, y1, x2, y2) {
      let result = []
      // 起始点
      result[0] = x1 + this.random(-this.offset, this.offset)
      result[1] = y1 + this.random(-this.offset, this.offset)
      // 终点
      result[2] = x2 + this.random(-this.offset, this.offset)
      result[3] = y2 + this.random(-this.offset, this.offset)
      // 两个控制点
      let c1 = this.getNearRandomPoint(x1, y1, x2, y2)
      let c2 = this.getNearRandomPoint(x1, y1, x2, y2)
      result[4] = c1[0]
      result[5] = c1[1]
      result[6] = c2[0]
      result[7] = c2[1]
      return result
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-04 17:08:21
     * @Desc: 画线段
     */
    drawLine (line, opt = {}) {
      this.ctx.save()
      this.ctx.beginPath()
      this.ctx.moveTo(line[0], line[1])
      this.ctx.bezierCurveTo(
        line[4],
        line[5],
        line[6],
        line[7],
        line[2],
        line[3]
      )
      this.ctx.lineWidth = opt.width || 1
      this.ctx.strokeStyle = opt.color || '#000'
      this.ctx.stroke()
      this.ctx.restore()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-13 18:01:07
     * @Desc: 计算两个点连成的线段上附近的一个随机点
     */
    getNearRandomPoint (x1, y1, x2, y2) {
      let xo, yo, rx, ry
      // 垂直x轴的线段特殊处理
      if (x1 === x2) {
        yo = y2 - y1
        rx = x1 + this.random(-2, 2)
        ry = y1 + yo * this.random(0, 1)
        return [rx, ry]
      }
      xo = x2 - x1
      rx = x1 + xo * this.random(0, 1)
      ry = ((rx - x1) * (y2 - y1)) / (x2 - x1) + y1
      ry += this.random(-2, 2)
      return [rx, ry]
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-05 11:04:01
     * @Desc: 绘制矩形
     */
    rectangle (x, y, width, height, opt = {}) {
      let points = [
        [x, y],
        [x + width, y],
        [x + width, y + height],
        [x, y + height]
      ]
      this.polygon(points, opt)
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-07 15:39:42
     * @Desc: 绘制多边形
     */
    polygon (points = [], opt = {}) {
      if (points.length < 3) {
        return
      }
      // 填充
      if (opt.fillType) {
        // 面积型
        if (opt.fillType === 'area') {
          let lines = this.closeLines(points)
          this.fillLines(lines, opt)
        } else if (opt.fillType === 'line') {
          this.strokeFillLines(points, opt)
        }
      }
      // 描边
      let len = points.length
      for (let i = 0; i < len - 1; i++) {
        this.line(
          points[i][0],
          points[i][1],
          points[i + 1][0],
          points[i + 1][1]
        )
      }
      this.line(
        points[len - 1][0],
        points[len - 1][1],
        points[0][0],
        points[0][1]
      )
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-13 16:09:41
     * @Desc: 圆
     */
    circle (x, y, r, opt) {
      // 填充
      if (opt.fillType) {
        // 面积型
        if (opt.fillType === 'area') {
          let lines = this.circleToLines(x, y, r, true)
          this.fillLines(lines, opt)
        } else if (opt.fillType === 'line') {
          let _points = this.circleToPoints(x, y, r, false, true)
          this.strokeFillLines(_points, opt)
        }
      }
      // 描边
      this.drawCircle(x, y, r)
      this.drawCircle(x, y, r)
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-20 11:28:07
     * @Desc: 把圆转换成多边形
     */
    circleToPoints (x, y, r, close, core) {
      let stepCount = 10
      let step = (2 * Math.PI) / stepCount
      let startOffset = -Math.PI / 2 + this.random(-Math.PI / 4, Math.PI / 4)
      let points = []
      let rx = r + this.random(-r * 0.05, r * 0.05)
      let ry = r + this.random(-r * 0.05, r * 0.05)
      for (
        let angle = startOffset;
        angle < 2 * Math.PI + startOffset;
        angle += step
      ) {
        let p = [
          x + rx * Math.cos(angle) + this.random(-2, 2),
          y + ry * Math.sin(angle) + this.random(-2, 2)
        ]
        points.push(p)
      }
      if (!core) {
        let end = [] // 处理最后一个连线点，让它和原本的点来点随机偏移
        if (close) {
          // 首尾闭合
          end[0] = points[1][0]
          end[1] = points[1][1]
        } else {
          // 不闭合
          let radRandom = step * this.random(0.1, 0.5)
          end[0] = x + rx * Math.cos(startOffset + step + radRandom)
          end[1] = y + ry * Math.sin(startOffset + step + radRandom)
        }
        points.push(
          [points[0][0], points[0][1]],
          [end[0], end[1]],
          [points[2][0], points[2][1]]
        )
      }
      return points
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 14:41:20
     * @Desc: 把圆转换成曲线
     */
    circleToLines (x, y, r, close = false) {
      let lines = []
      // 圆变多边形
      let points = this.circleToPoints(x, y, r, close)
      // 线段变曲线
      let len = points.length
      for (let i = 1; i + 2 < len; i++) {
        let c1, c2, c3
        let point = points[i]
        let num = 6
        c1 = [
          point[0] + (points[i + 1][0] - points[i - 1][0]) / num,
          point[1] + (points[i + 1][1] - points[i - 1][1]) / num
        ]
        c2 = [
          points[i + 1][0] + (point[0] - points[i + 2][0]) / num,
          points[i + 1][1] + (point[1] - points[i + 2][1]) / num
        ]
        c3 = [points[i + 1][0], points[i + 1][1]]
        lines.push([
          // 起点
          point[0],
          point[1],
          // 终点
          c3[0],
          c3[1],
          // 两个控制点
          c1[0],
          c1[1],
          c2[0],
          c2[1]
        ])
      }
      return lines
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-18 20:18:32
     * @Desc: 绘制单线圆
     */
    drawCircle (x, y, r) {
      let lines = this.circleToLines(x, y, r)
      this.ctx.beginPath()
      this.ctx.strokeStyle = '#000'
      for (let i = 0; i < lines.length; i++) {
        let line = lines[i]
        if (i === 0) {
          this.ctx.moveTo(line[0], line[1])
        }
        this.ctx.bezierCurveTo(
          line[4],
          line[5],
          line[6],
          line[7],
          line[2],
          line[3]
        )
      }
      this.ctx.stroke()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-05 10:02:26
     * @Desc: 绘制点
     */
    drawPoint (x, y, color) {
      this.ctx.beginPath()
      this.ctx.arc(x, y, 2, 0, 2 * Math.PI)
      this.ctx.fillStyle = color || 'red'
      this.ctx.fill()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-04 16:35:58
     * @Desc: 返回一个范围内的随机数
     */
    random (min, max) {
      return Math.random() * (max - min) + min
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 13:43:28
     * @Desc: 转换成首尾相连的曲线段
     */
    closeLines (points) {
      let len = points.length
      let lines = []
      let lastPoint = null
      for (let i = 0; i < len - 1; i++) {
        let arr = this._line(
          points[i][0],
          points[i][1],
          points[i + 1][0],
          points[i + 1][1]
        )
        lines.push([
          lastPoint ? lastPoint[2] : arr[0], // 上一个点存在则使用上一个点的终点来作为该点的起点
          lastPoint ? lastPoint[3] : arr[1],
          arr[2],
          arr[3],
          arr[4],
          arr[5],
          arr[6],
          arr[7]
        ])
        lastPoint = arr
      }
      let arr = this._line(
        points[len - 1][0],
        points[len - 1][1],
        points[0][0],
        points[0][1]
      )
      lines.push([
        lastPoint ? lastPoint[2] : arr[0], // 上一个点存在则使用上一个点的终点来作为该点的起点
        lastPoint ? lastPoint[3] : arr[1],
        lines[0][0], // 终点是第一条线段的起点
        lines[0][1],
        arr[4],
        arr[5],
        arr[6],
        arr[7]
      ])
      return lines
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 20:21:35
     * @Desc: 把线段转换成首尾相连的曲线段
     */
    linesToLinkTines (lines) {
      // 先把线段转换成曲线
      let curveLines = []
      let preLine = null
      let tail = false
      lines.forEach((line) => {
        let _line
        if (tail) {
          _line = this._line(
            preLine ? preLine[1][0] : line[0][0],
            preLine ? preLine[1][1] : line[0][1],
            preLine ? line[0][0] : line[1][0],
            preLine ? line[0][1] : line[1][1]
          )
        } else {
          _line = this._line(
            preLine ? preLine[0][0] : line[0][0],
            preLine ? preLine[0][1] : line[0][1],
            preLine ? line[1][0] : line[1][0],
            preLine ? line[1][1] : line[1][1]
          )
        }
        curveLines.push(_line)
        preLine = line
        tail = !tail
      })
      return curveLines
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-20 14:20:02
     * @Desc: 线段填充，支持旋转角度
     */
    strokeFillLines (points, opt) {
      // 多边形旋转指定角度
      if (opt.rotate) {
        points = this.rotatePoints(points, opt.rotate)
      }
      // 线段型
      let lines = this.scanLines(points, opt)
      // 线段再旋转回去
      if (opt.rotate) {
        lines = this.rotateLines(lines, -opt.rotate)
      }
      // 一笔画型
      if (opt.tailLink) {
        let tailLines = this.linesToLinkTines(lines)
        this.strokeLines(tailLines, {
          color: opt.fillStyle,
          width: opt.fillLineWidth
        })
      } else {
        // 平行型
        lines.forEach((line) => {
          this[opt.doubleLine ? 'drawDoubleLine' : 'drawSingleLine'](
            line[0][0],
            line[0][1],
            line[1][0],
            line[1][1],
            {
              color: opt.fillStyle,
              width: opt.fillLineWidth
            }
          )
        })
      }
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 13:52:14
     * @Desc: 填充多边形
     */
    fillLines (lines, opt) {
      this.ctx.beginPath()
      this.ctx.fillStyle = opt.fillStyle
      for (let i = 0; i + 1 < lines.length; i++) {
        let line = lines[i]
        if (i === 0) {
          this.ctx.moveTo(line[0], line[1])
        }
        this.ctx.bezierCurveTo(
          line[4],
          line[5],
          line[6],
          line[7],
          line[2],
          line[3]
        )
      }
      this.ctx.fill()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 20:40:26
     * @Desc: 描边线段
     */
    strokeLines (lines, opt) {
      this.ctx.save()
      this.ctx.beginPath()
      this.ctx.lineJoin = 'round'
      this.ctx.lineWidth = opt.width
      this.ctx.strokeStyle = opt.color
      for (let i = 0; i < lines.length; i++) {
        let line = lines[i]
        if (i === 0) {
          this.ctx.moveTo(line[0], line[1])
        }
        this.ctx.bezierCurveTo(
          line[4],
          line[5],
          line[6],
          line[7],
          line[2],
          line[3]
        )
      }
      this.ctx.stroke()
      this.ctx.restore()
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-19 18:49:05
     * @Desc:  创建排序边表ET
     */
    createEdgeTable (points) {
      // 创建边表ET
      let edgeTable = []
      // 将第一个点复制一份到队尾，用来闭合多边形
      let _points = points.concat([[points[0][0], points[0][1]]])
      let len = _points.length
      for (let i = 0; i < len - 1; i++) {
        let p1 = _points[i]
        let p2 = _points[i + 1]
        // 过滤掉平行于x轴的线段
        if (p1[1] !== p2[1]) {
          let ymin = Math.min(p1[1], p2[1])
          edgeTable.push({
            ymin,
            ymax: Math.max(p1[1], p2[1]),
            xi: ymin === p1[1] ? p1[0] : p2[0], // 最低顶点的x值
            dx: (p2[0] - p1[0]) / (p2[1] - p1[1]) // 线段的斜率的倒数
          })
        }
      }
      // 对边表进行排序
      edgeTable.sort((e1, e2) => {
        // 按ymin递增排序
        if (e1.ymin < e2.ymin) {
          return -1
        }
        if (e1.ymin > e2.ymin) {
          return 1
        }
        // ymin相同则按xi递增
        if (e1.xi < e2.xi) {
          return -1
        }
        if (e1.xi > e2.xi) {
          return 1
        }
        // xi也相同则只能看ymax
        // ymax还相同，说明两条边重合
        if (e1.ymax === e2.ymax) {
          return 0
        }
        // 如果不重合，则按yamx递增排序
        if (e1.ymax < e2.ymax) {
          return -1
        }
        if (e1.ymax > e2.ymax) {
          return 1
        }
      })
      return edgeTable
    },

    /**
     * @Author: 王林25
     * @Date: 2021-02-07 15:31:18
     * @Desc: 扫描填充算法获取填充线段
     */
    scanLines (points, opt = {}) {
      console.log(JSON.stringify(points))
      if (points.length < 3) {
        return []
      }
      opt.gap = opt.gap || 1
      let lines = []
      // 创建排序边表ET
      let edgeTable = this.createEdgeTable(points)
      // debugger
      // 活动边表AET
      let activeEdgeTable = []
      // 开始扫描，从多边形的最低点开始
      let y = edgeTable[0].ymin
      // 循环的终点是两个表都为空
      while (edgeTable.length > 0 || activeEdgeTable.length > 0) {
        // 从ET表里把当前扫描线的边添加到AET表里
        if (edgeTable.length > 0) {
          // 将当前ET表里和扫描线相交的边添加到AET表里
          for (let i = 0; i < edgeTable.length; i++) {
            if (
              (edgeTable[i].ymin <= y && edgeTable[i].ymax >= y) ||
              edgeTable[i].ymax < y
            ) {
              let removed = edgeTable.splice(i, 1)
              activeEdgeTable.push(...removed)
              i--
            }
          }
        }
        // 从AET表里删除y=ymax的记录
        activeEdgeTable = activeEdgeTable.filter((item) => {
          return y < item.ymax
        })
        // 按xi从小到大排序
        activeEdgeTable.sort((e1, e2) => {
          if (e1.xi < e2.xi) {
            return -1
          } else if (e1.xi > e2.xi) {
            return 1
          } else {
            return 0
          }
        })
        // 如果存在活动边，则填充活动边之间的区域
        if (activeEdgeTable.length > 1) {
          // 每次取两个边出来进行填充
          for (let i = 0; i + 1 < activeEdgeTable.length; i += 2) {
            lines.push([
              [Math.round(activeEdgeTable[i].xi), y],
              [Math.round(activeEdgeTable[i + 1].xi), y]
            ])
          }
        }
        // 更新活动边的xi
        activeEdgeTable.forEach((item) => {
          item.xi += item.dx * opt.gap
        })
        // 更新扫描线y
        y += opt.gap
      }
      return lines
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-20 10:37:22
     * @Desc: 旋转顶点列表
     */
    rotatePoints (points, rotate) {
      return points.map((item) => {
        return this.getRotatedPos(item[0], item[1], rotate)
      })
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-20 10:57:18
     * @Desc: 旋转线段列表
     */
    rotateLines (lines, rotate) {
      return lines.map((line) => {
        return [
          this.getRotatedPos(line[0][0], line[0][1], rotate),
          this.getRotatedPos(line[1][0], line[1][1], rotate)
        ]
      })
    },

    /**
     * @Author: 王林25
     * @Date: 2021-03-20 10:30:31
     * @Desc:  求一个点旋转指定角度后的坐标
     */
    getRotatedPos (x, y, rad) {
      rad = (Math.PI / 180) * rad
      return [
        x * Math.cos(rad) - y * Math.sin(rad),
        y * Math.cos(rad) + x * Math.sin(rad)
      ]
    }
  }
}
</script>

<style lang="less">
.container {
  width: 100%;
  height: 100%;

  .canvas {
    width: auto;
    height: auto;
    border: 1px solid #999;
  }

  .btn {
    display: flex;
    width: 1000px;
    justify-content: center;
  }
}
</style>
