## 变形折叠动画
```xml
<template>

  <div>

    <div class="collapsablePane">

      <div class="header">

        <div>静夜思</div>

        <button type="button" class="collapseBtn">

          <span class="text">展开文本</span>

          <svg class="arrow" viewBox="0 0 160 160" xmlns="https://www.w3.org/2000/svg">

            <polyline class="arrow-polyline" points="20,50 80,110 140,50">

              <animate

                class="arrow-animate"

                attributeName="points"

                dur="0.2s"

                fill="freeze"

                restart="whenNotActive"

              ></animate>

            </polyline>

          </svg>

        </button>

      </div>

  

      <div class="content">

        <p>窗前明月光，</p>

        <p>疑似地上霜。</p>

        <p>举头望明月，</p>

        <p>低头思故乡。</p>

      </div>

    </div>

  </div>

</template>
```

```js
<script setup>

let isCollapse = true;

  

const textEle = document.querySelector(".text");

  

function updateText() {

  textEle.textContent = isCollapse ? "展开文本" : "折叠文本";

}

updateText();

  

const collapseBtn = document.querySelector(".collapseBtn");

const paneContent = document.querySelector(".collapsablePane > .content");

  

const animate = collapseBtn.querySelector(".arrow-animate");

  

animate.onend = () => {

  updateText();

};

  

collapseBtn.addEventListener("click", event => {

  isCollapse = !isCollapse;

  if (isCollapse) {

    animate.setAttribute("from", "20,110 80,50 140,110");

    animate.setAttribute("to", "20,50 80,110 140,50");

  } else {

    animate.setAttribute("from", "20,50 80,110 140,50");

    animate.setAttribute("to", "20,110 80,50 140,110");

  }

  animate.beginElement();

  paneContent.classList.toggle("unfold");

});

</script>
```
```css
<style lang="scss" scoped>

.collapsablePane {

  width: 500px;

  max-width: 100%;

  border-radius: 3px;

  overflow: hidden;

}

  

.collapsablePane > .header {

  display: flex;

  justify-content: space-between;

  padding: 0.5rem 1rem;

  align-items: center;

  background-color: #353535;

  color: #fff;

  overflow: hidden;

}

  

.collapsablePane > .content {

  padding-inline: 1rem;

  height: 0;

  background-color: #f0f0f0;

  overflow: hidden;

  transition: height 0.2s linear;

  box-sizing: border-box;

}

  

.collapsablePane > .content.unfold {

  height: 180px;

}

  

.collapseBtn {

  display: flex;

  align-items: center;

  background-color: transparent;

  border-color: transparent;

  color: #0095ff;

  outline: none;

  cursor: pointer;

  transition: background-color 0.2s linear;

}

  

.collapseBtn:hover {

  background-color: #393939;

}

  

.arrow {

  width: 1em;

  height: 1em;

  margin-left: 0.25rem;

}

  

.arrow-polyline {

  fill: none;

  stroke: currentColor;

  stroke-width: 12;

  stroke-linecap: round;

  stroke-linejoin: round;

}

</style>
```
https://code.juejin.cn/pen/7392889728112164874