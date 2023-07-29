<template>
    <div class="container">
          <div class="gauge-bg"></div>
          <div class="gauge-middle"></div>
          <div class="gauge-overlay" :style="{ transform: rotate }"></div>
          <div class="gauge-data">
            <!-- <span>{{ percentage }}%</span> -->
          </div>
        
        <div class="pointer" :style="{ transform: pointerTransform }">
        </div>
    </div>
    </template>
    
    <script>
    export default {
      props: ['percentage'],
      computed: {
        rotate() {
          const v = this.percentage * 180 / 100
          return `rotate(${v}deg)`
        },
        pointerTransform() {
            const v = this.percentage * 180 / 100;
            return `rotate(${v}deg)`;
    },
      }
    
    }
    </script>
    
    <style>
    
    .container {
      width: 400px;
      height: 200px;
      position: absolute;
      top: 50;
      overflow: hidden;
    }
    .pointer {
        z-index: 5; /* Ensure the pointer appears above other elements */
        position: absolute;
        width: 12px;
        height: 135px;
        background-color: black; /* Customize the pointer appearance */
        top: 30%;
        left: calc(50% - 3px);
        border-top-left-radius: 100%;
        border-top-right-radius: 100%;
        border-bottom-left-radius: 20%;
        border-bottom-right-radius: 20%;
        transform-origin: bottom center;
        rotate: -90deg;
        }
        /* .pointer{
            position: relative;
            top: 8%;
            left:47%;
            width: 0; 
            height: 0; 
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 140px solid blue;
            border-bottom-left-radius: 35%;
            border-bottom-right-radius: 35%;
            transform-origin: center bottom;
            transform: rotate(-125deg);
            } */
    .gauge-bg {
      z-index: 1;
      width: 400px;
      height: 200px;
      position: absolute;
      background: linear-gradient(to right, #df0000 0%, #ff6262 50%, #1bff1f 50%, #00b603 100%);
      /* background-color: rgba(red, green, blue, alpha); */
      border-radius: 250px 250px 0 0;
    }
    .gauge-middle {
      z-index: 3;
      position: absolute;
      background-color: #fff;
      width: 325px;
      height: calc(370px / 2);
      top: 30px;
      margin-left: 15px;
      margin-right: auto;
      border-radius: 250px 250px 0 0px;
    }
    .gauge-overlay {
      z-index: 2;
      position: absolute;
      /* background-color: #0a1206; */
      width: 400px;
      height: 200px;
      top: 200px;
      border-radius: 0 0 200px 200px;
      transform-origin: center top;
    }
    .gauge-data {
      z-index: 4;
      color: #5df086;
      position: absolute;
      width: 400px;
      bottom: 0;
      text-align: center;
      font-size: 24px;
    }
    
    </style>