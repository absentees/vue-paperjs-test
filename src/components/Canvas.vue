<template>
        <canvas
            :id="canvasId"
            class="canvas-style"
            v-on:mousedown="mouseDown"
        />
</template>

<script>
const paper = require("paper");
export default {
    name: "Canvas",
    props: ["canvasId","inputText"],
    watch: {
        inputText: {
            handler(){
                this.text.content = this.inputText;
            }
        }
    },
    data: () => ({
        path: null,
        path2: null,
        scope: null,
        text: null,
        bgImage: null
    }),
    methods: {
        reset() {
            this.scope.project.activeLayer.removeChildren();
        },
        pathCreate(scope) {
            scope.activate();
            return new paper.Path({
                strokeColor: "#000000",
                strokeJoin: "round",
                strokeWidth: 1.5,
            });
        },
        createTool(scope) {
            scope.activate();
            return new paper.Tool();
        },
        mouseDown() {
            // in order to access functions in nested tool
            let self = this;
            self.scope.activate();
            // create drawing tool
            this.tool = this.createTool(this.scope);
            this.tool.minDistance = 40;
            this.tool.onMouseDown = (event) => {
                // init path
                self.path = self.pathCreate(self.scope);
                // add point to path
                self.path.add(event.point);

                self.path2 = self.pathCreate(this.scope);
                self.path2.add(event.point);
            };
            this.tool.onMouseDrag = (event) => {
                self.path.add(event);
                self.path2.arcTo(event.point);
            };
            this.tool.onMouseUp = (event) => {
                // line completed
                self.path.add(event.point);
            };
        },
    },
    mounted() {
        this.scope = new paper.PaperScope();
        this.scope.setup(this.canvasId);
        this.text = new paper.PointText({
            point: [50, 50],
            content: this.inputText,
            fillColor: "black",
            fontFamily: "Courier New",
            fontWeight: "bold",
            fontSize: 25,
        });

        this.bgImage = new paper.Raster({
            source: "https://images.unsplash.com/photo-1527443060795-0402a18106c2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=751&q=80",
            position: new paper.Point(100,100)
            // position: paper.view.center
        });

        console.log(paper.scope.view)

// https://images.unsplash.com/photo-1527443060795-0402a18106c2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=751&q=80

    },
};
</script>

<style scoped>
.canvas-style {
    cursor: crosshair;
    width: 90vw !important;
    height: 90vh !important;
    border: 1px solid black;
    border-radius: 4px;
    display: block;
    margin: auto;
    margin-top: 5vh;
}
</style>
