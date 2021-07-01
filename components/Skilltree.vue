<template>
  <v-container>
    <h1>Professional Skills</h1>

    <v-container class="d-flex flex-column align-center">
      <v-card :width="card_width" class="ma-auto">
        <v-row class="ma-10">
          <v-col cols="12" sm="12" md="8">
            <v-row class="mt-4">
              <v-avatar size="70">
                <img
                  :src="
                    selectedSkill.img_url
                      ? selectedSkill.img_url
                      : '/nuxt-web/bga.jpg'
                  "
                />
              </v-avatar>
              <v-card-title>{{ selectedSkill.title }}</v-card-title>
            </v-row>
          </v-col>
          <v-col cols="12" sm="12" md="4">
            <h4 class="mt-6">{{ selectedSkill.level }}</h4>
            <v-progress-linear
              :value="
                selectedSkill.level
                  ? this.levelProperty(selectedSkill.level, 'percentage')
                  : 0
              "
              :color="
                selectedSkill.level
                  ? this.levelProperty(selectedSkill.level, 'color')
                  : 'black'
              "
              buffer-value="100"
            ></v-progress-linear>
          </v-col>
        </v-row>
        <v-card-subtitle>Description</v-card-subtitle>
        <v-card-text>{{ selectedSkill.description }}</v-card-text>
      </v-card>
        <svg id="svg" class="my-5" :width="svg_width"></svg>
    </v-container>
  </v-container>
</template>
<script>
import {
  select,
  hierarchy,
  tree,
  linkVertical,
  event,
  linkHorizontal,
} from 'd3'
// import eventlistener from '~/plugins/move.js'

export default {
  mounted() {
    // ## AddEventListener ##
    this.svg_tag = document.querySelector('#svg')
    const svgtg = this.svg_tag
    if (svgtg) {
      if (window.PointerEvent) {
        svgtg.addEventListener('pointerdown', this.onPointerDown) // Pointer is pressed
        svgtg.addEventListener('pointerup', this.onPointerUp) // Releasing the pointer
        svgtg.addEventListener('pointerleave', this.onPointerUp) // Pointer gets out of the SVG area
        svgtg.addEventListener('pointermove', this.onPointerMove) // Pointer is moving
        svgtg.addEventListener('wheel', this.zoom)
      } else {
        // Add all mouse events listeners fallback
        svgtg.addEventListener('mousedown', onPointerDown) // Pressing the mouse
        svgtg.addEventListener('mouseup', onPointerUp) // Releasing the mouse
        svgtg.addEventListener('mouseleave', onPointerUp) // Mouse gets out of the SVG area
        svgtg.addEventListener('mousemove', onPointerMove) // Mouse is moving

        // Add all touch events listeners fallback
        svgtg.addEventListener('touchstart', onPointerDown) // Finger is touching the screen
        svgtg.addEventListener('touchend', onPointerUp) // Finger is no longer touching the screen
        svgtg.addEventListener('touchmove', onPointerMove) // Finger is moving
      }
    }

    // access the root node
    const dx = 100
    const dy = 80
    const diagonal = linkVertical()
      .x((d) => d.x)
      .y((d) => d.y)
    const root = hierarchy(this.treeData)
    root.x0 = 300
    root.y0 = 0
    // console.log("root", root)

    root.descendants().forEach((d, i) => {
      d.id = i
      d._children = d.children
      if (d.depth && d.data.name.length !== 7) d.children = null
    })
    console.log('root', root)
    const margin = { top: 100, right: 120, bottom: 100, left: 120 }
    const height = 500
    let width = 1000
    const svg = select('svg')
      .attr('viewBox', [-margin.left, -margin.top, width, height])
      // .attr("viewport", [width, height])
      .style('font', '10px sans-serif')
      .style('user-select', 'none')

    this.viewBox = {
      x: -margin.left,
      y: -margin.top,
      width,
      height,
    }
    const gLink = svg
      .append('g')
      .attr('fill', 'none')
      .attr('stroke', '#555')
      .attr('stroke-opacity', 0.4)
      .attr('stroke-width', 'none')

    const gNode = svg
      .append('g')
      .attr('cursor', 'pointer')
      .attr('pointer-events', 'all')

    console.log(this.viewBox)
    // console.log("gLInk", gLink)
    // const height = document.body.clientHeight - 500
    // const width = document.body.clientWidth - 500
    this.point = this.svg_tag.createSVGPoint()
    this.update(root, root, dx, dy, height, margin, svg, gNode, gLink, diagonal)
  },
  computed: {
    svg_width() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return 300
        case 'sm':
          return 500
        case 'md':
          return 800
        case 'lg':
          return 1000
      }
    },
    card_width() {
      switch (this.$vuetify.breakpoint.name) {
        case 'xs':
          return 300
        case 'sm':
          return 400
        case 'md':
          return 500
        case 'lg':
          return 600
      }
    }
  },
  methods: {
    levelProperty: function (level, prop) {
      const levels = {
        Basic: { percentage: 33, color: 'orange' },
        Familiar: { percentage: 66, color: 'green' },
        Expert: { percentage: 100, color: 'red lighten-2' },
      }
      return levels[level][prop]
    },
    update: function (
      source,
      root,
      dx,
      dy,
      height,
      margin,
      svg,
      gNode,
      gLink,
      diagonal
    ) {
      // console.log("source", source)
      // console.log("root", root)
      const duration = 250
      const nodes = root.descendants()
      const links = root.links()

      const treeLayout = tree().nodeSize([dx, dy])
      treeLayout(root)
      // console.log("root2", root)

      let left = root
      let right = root
      root.eachBefore((node) => {
        if (node.x < left.x) left = node
        if (node.x > right.x) right = node
      })

      const width = right.x - left.x + margin.left + margin.right

      const transition = svg
        .transition()
        .duration(duration)
        .attr('viewBox', [-margin.left + left.x, -margin.top, width, height])
        .tween(
          'resize',
          window.ResizeObserver ? null : () => () => svg.dispatch('toggle')
        )
      console.log(this.viewBox)
      // console.log("box", this.viewBox)
      // console.log(viewBox)

      this.viewBox.x = -margin.left + left.x
      this.viewBox.y = -margin.top
      this.viewBox.width = width
      this.viewBox.height = height

      // Update the nodes…
      const node = gNode.selectAll('g').data(nodes, (d) => d.id)

      // Enter any new nodes at the parent's previous position.
      const nodeEnter = node
        .enter()
        .append('g')
        .attr('transform', (d) => `translate(${source.x0},${source.y0})`)
        .attr('fill-opacity', 0)
        .attr('stroke-opacity', 0)
        .on('click', (event, d) => {
          d.children = d.children ? null : d._children
          if (!d.data.children) {
            console.log('data', d.data)
            this.selectedSkill = {
              img_url: d.data.img,
              title: d.data.name,
              description: d.data.description,
              level: d.data.level,
            }
          }
          console.log('Dnode', d)
          this.update(
            d,
            root,
            dx,
            dy,
            height,
            margin,
            svg,
            gNode,
            gLink,
            diagonal
          )
        })
      // console.log({nodeEnter})
      const radius = 35

      const pattern = nodeEnter
        .append('defs')
        .append('pattern')
        .attr('id', (d) => 'img' + d.id)
        .attr('patternUnits', 'objectBoundingBox')
        .attr('height', 1)
        .attr('width', 1)
      pattern
        .append('image')
        .attr('x', 0)
        .attr('y', 0)
        .attr('height', 2 * radius)
        .attr('width', 2 * radius)
        .attr('xlink:href', (d) => (d._children ? '' : d.data.img))

      nodeEnter
        .append('circle')
        .attr('r', (d) => (d._children ? 0 : radius))
        .attr('cx', 0)
        .attr('cy', (d) => (d._children ? 0 : radius))
        .attr('fill', (d) => (d._children ? '#555' : `url(#img${d.id})`))
        .attr('stroke', 'orange')
        .attr('stroke-width', 1)

      const rect_width = 70
      const rect_height = 30
      nodeEnter
        .append('rect')
        .attr('x', -rect_width / 2)
        .attr('rx', 10)
        .attr('width', (d) => (d._children ? rect_width : 0))
        .attr('height', (d) => (d._children ? rect_height : 0))
        .attr('fill', '#F2E855')
        .attr('stroke', 'black')
        .attr('stroke-width', 0.2)

      nodeEnter
        .append('text')
        .attr('y', rect_height / 2 + 5)
        .attr('x', -rect_width / 2 + 5)
        // .attr("x", d => d._children ? -6 : 6)
        // .attr("text-anchor", d => d._children ? "end" : "start")
        .attr('font-size', 'medium')
        .attr('font-family', 'monospace')
        .text((d) => (d._children ? d.data.name : ''))
      // .clone(true).lower()
      //     .attr("stroke-linejoin", "round")
      //     .attr("stroke-width", 3)
      //     .attr("stroke", "white");

      // Transition nodes to their new position.
      const nodeUpdate = node
        .merge(nodeEnter)
        .transition(transition)
        .attr('transform', (d) => `translate(${d.x},${d.y})`)
        .attr('fill-opacity', 1)
        .attr('stroke-opacity', 1)

      // Transition exiting nodes to the parent's new position.
      const nodeExit = node
        .exit()
        .transition(transition)
        .remove()
        .attr('transform', (d) => `translate(${source.x},${source.y})`)
        .attr('fill-opacity', 0)
        .attr('stroke-opacity', 0)

      // Update the links…
      const link = gLink.selectAll('path').data(links, (d) => d.target.id)

      // Enter any new links at the parent's previous position.
      const linkEnter = link
        .enter()
        .append('path')
        .attr('d', (d) => {
          const o = { x: source.x0, y: source.y0 }
          return diagonal({ source: o, target: o })
        })
        .attr('stroke', 'orange')
        .attr('stroke-width', 3)

      // Transition links to their new position.
      link.merge(linkEnter).transition(transition).attr('d', diagonal)

      // Transition exiting nodes to the parent's new position.
      link
        .exit()
        .transition(transition)
        .remove()
        .attr('d', (d) => {
          const o = { x: source.x, y: source.y }
          return diagonal({ source: o, target: o })
        })

      // Stash the old positions for transition.
      root.eachBefore((d) => {
        d.x0 = d.x
        d.y0 = d.y
      })
    },
    getPointFromEvent: function (event) {
      let point = { x: 0, y: 0 }
      // If event is triggered by a touch event, we get the position of the first finger
      if (event.targetTouches) {
        point.x = event.targetTouches[0].clientX
        point.y = event.targetTouches[0].clientY
      } else {
        point.x = event.clientX
        point.y = event.clientY
      }
      // let invertedSVGMatrix = this.svg_tag.getScreenCTM().inverse()
      // return this.point.matrixTransform(invertedSVGMatrix);
      return point
    },
    zoom: function (event) {
      console.log('Enter zoom')
      let r = 1
      if (event.deltaY > 0) {
        r = 0.9
      } else if (event.deltaY < 0) {
        r = 1.1
      }
      const viewBoxString = `${this.viewBox.x} ${this.viewBox.y} ${
        this.viewBox.width * r
      } ${this.viewBox.height * r}`
      this.viewBox.width *= r
      this.viewBox.height *= r
      this.svg_tag.setAttribute('viewBox', viewBoxString)
    },
    onPointerDown: function (event) {
      this.isPointerDown = true // We set the pointer as down

      // We get the pointer position on click/touchdown so we can get the value once the user starts to drag
      const pointerPosition = this.getPointFromEvent(event)
      this.pointerOrigin.x = pointerPosition.x
      this.pointerOrigin.y = pointerPosition.y
    },
    onPointerMove: function (event) {
      // Only run this function if the pointer is down
      if (!this.isPointerDown) {
        return
      }
      // This prevent user to do a selection on the page
      event.preventDefault()

      // Get the pointer position
      const pointerPosition = this.getPointFromEvent(event)

      // We calculate the distance between the pointer origin and the current position
      // The viewBox x & y values must be calculated from the original values and the distances
      this.newViewBox.x =
        this.viewBox.x - (pointerPosition.x - this.pointerOrigin.x)
      this.newViewBox.y =
        this.viewBox.y - (pointerPosition.y - this.pointerOrigin.y)
      // newViewBox.x = viewBox.x - ((pointerPosition.x - pointerOrigin.x) * ratio);
      // newViewBox.y = viewBox.y - ((pointerPosition.y - pointerOrigin.y) * ratio);

      // We create a string with the new viewBox values
      // The X & Y values are equal to the current viewBox minus the calculated distances
      const viewBoxString = `${this.newViewBox.x} ${this.newViewBox.y} ${this.viewBox.width} ${this.viewBox.height}`
      // We apply the new viewBox values onto the SVG
      this.svg_tag.setAttribute('viewBox', viewBoxString)
    },
    onPointerUp: function () {
      // The pointer is no longer considered as down
      this.isPointerDown = false

      // We save the viewBox coordinates based on the last pointer offsets
      this.viewBox.x = this.newViewBox.x
      this.viewBox.y = this.newViewBox.y
    },
  },
  data() {
    return {
      selectedSkill: {
        img_url: '',
        description: '',
        title: 'Select a skill',
        level: '',
      },
      svg_tag: null,
      point: null,
      isPointerDown: false,
      pointerOrigin: {
        x: 0,
        y: 0,
      },
      viewBox: {
        x: 0,
        y: 0,
        width: 500,
        height: 500,
      },
      newViewBox: {
        x: 0,
        y: 0,
      },
      treeData: {
        name: 'Skills',
        children: [
          {
            name: 'Frontend',
            children: [
              {
                name: 'React.js',
                img: '/nuxt-web/logos/react.svg',
                level: 'Basic',
                description: 'React',
              },
              {
                name: 'Vue.js',
                img: '/nuxt-web/logos/vue.png',
                level: 'Basic',
                description: 'Vue',
              },
            ],
          },
          {
            name: 'Backend',
            children: [
              {
                name: 'Flask',
                img: '/nuxt-web/logos/flask.png',
                level: 'Familiar',
                description: 'Flask',
              },
              {
                name: 'FastAPI',
                img: '/nuxt-web/logos/fastapi.jpg',
                level: 'Familiar',
                description: 'FastAPI',
              },
              {
                name: 'Golang',
                img: '/nuxt-web/logos/golang.jpg',
                level: 'Basic',
                description: 'Golang',
              },
            ],
          },
          {
            name: 'Dev-ops',
            children: [
              {
                name: 'Docker',
                img: '/nuxt-web/logos/docker.jpg',
                level: 'Expert',
                description: 'Docker',
              },
              {
                name: 'Kubernetes',
                img: '/nuxt-web/logos/k8s.png',
                level: 'Basic',
                description: 'K8s',
              },
              {
                name: 'Ansible',
                img: '/nuxt-web/logos/ansible.png',
                level: 'Basic',
                description: 'Ansible',
              },
            ],
          },
          {
            name: 'Data',
            children: [
              {
                name: 'Pytorch',
                img: '/nuxt-web/logos/pytorch.png',
                level: 'Familiar',
                description: 'Pytorch',
              },
              {
                name: 'keras',
                img: '/nuxt-web/logos/keras.png',
                level: 'Basic',
                description: 'keras',
              },
            ],
          },
          {
            name: 'Others',
            children: [
              {
                name: 'Git',
                img: '/nuxt-web/logos/git.png',
                level: 'Familiar',
                description: 'Git',
              },
              {
                name: 'C++',
                img: '/nuxt-web/logos/k8s.png',
                level: 'Basic',
                description: 'K8s',
              },
            ],
          },
        ],
      },
    }
  },
}
</script>
<style>
svg {
  outline: 1px solid white;
  height: 500px;
  cursor: move;
  touch-action: none;
  background-color: #2d2a2a;
}
</style>
