<template>
    <Page>
        <ActionBar>
            <NavigationButton text="Back" android.systemIcon="ic_menu_back" @tap="onBack"></NavigationButton>
            <StackLayout orientation="horizontal">
                <Button text="color" @tap="changeColor" />
                <Button text="size" @tap="animateRectSize" />
                <Button text="arc" @tap="animateArc" />
            </StackLayout>

        </ActionBar>
        <GridLayout rows="*,auto">
            <CanvasView row="0" :density="density" ref="canvas" width="100%" backgroundColor="#44ff0000">
                <Shapes>
                    <Rectangle shadow="3 3 black" strokeColor="red" :fillColor="shapeColor" strokeWidth="6" :left="shapeLeft" top="10" :width="shapeLeft + '%'" height="50%" />
                    <Arc color="yellow" :antiAlias="antiAlias" shadow="0 0 10 red" paintStyle="stroke" strokeCap="round" strokeWidth="10" left="20" top="20" :width="arcWidth" :height="arcWidth" startAngle="0" :sweepAngle="sweepAngle" />
                    <Text  :antiAlias="antiAlias" color="green" top="50" text="test Text" fontSize="50" />
                    <Line  :antiAlias="antiAlias" color="yellow"  startX="10" startY="10" stopX="505" stopY="50"  strokeCap="round"  strokeJoin="round" strokeWidth="8" dash="1 10 0"/>
                    <Line  :antiAlias="antiAlias" color="#1C2738"  startX="10%" startY="50%" stopX="90%" stopY="50%"  strokeCap="round"  strokeJoin="round" strokeWidth="1" dash="1 3 0"/>
                </Shapes>
            </CanvasView>
            <GridLayout row="1" columns="auto, *, auto" orientation="horizontal">
                <Label col="0" text="density" verticalAlignment="center"/>
                <Slider col="1" height="40" v-model="density" minValue="0" maxValue="10" verticalAlignment="center" />
                <Switch col="2" v-model="antiAlias"/>
            </GridLayout>
        </GridLayout>
    </Page>
</template>

<script lang="ts">
import * as frameModule from 'tns-core-modules/ui/frame';
import Vue from 'nativescript-vue';
import { Component } from 'vue-property-decorator';
import { Canvas, createRect, Paint, Style } from 'nativescript-canvas';
import { Color } from 'tns-core-modules/color/color';
import * as Anim from '../animation';
import { screen } from 'tns-core-modules/platform';

@Component
export default class Simple extends Vue {
    shapeColor = 'blue';
    shapeLeft = 10;
    sweepAngle = 0;
    arcWidth = Math.min(screen.mainScreen.widthDIPs, screen.mainScreen.heightDIPs) - 40;
    density = screen.mainScreen.scale
    antiAlias = true
    static title: 'Shapes Example';
    mounted() {}
    onBack() {
        frameModule.topmost().goBack();
    }
    changeColor() {
        this.shapeColor = this.shapeColor === 'blue' ? 'yellow' : 'blue';
    }
    animateRectSize() {
        new Anim.Animation({ value: 10 })
            .to({ value: 40 }, 5000)
            .easing(Anim.Easing.Quadratic.Out)
            .onUpdate(obj => {
                this.shapeLeft = obj.value;
            })
            .start();
    }
    animateArc() {
        new Anim.Animation({ value: 0 })
            .to({ value: 360 }, 5000)
            .onUpdate(obj => {
                this.sweepAngle = obj.value;
            })
            .start();
    }

    onDraw(event: { canvas: Canvas }) {
        const canvas = event.canvas;
        const w = canvas.getWidth();
        const h = canvas.getHeight();
        // console.log('onDraw', w, h);
    }
}
</script>
