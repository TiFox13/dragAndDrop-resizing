<script setup>
import {onMounted, ref} from 'vue'
import DraggableResizableVue from 'draggable-resizable-vue3'

onMounted(() => {

  const moveBlock = document.querySelector('.block-move')
const ball = document.querySelector('.myNote')
ball.onmousedown = function(event) {
  event.target.classList.add('active')
  ball.querySelector('.handles').classList.add('block-active')
//   // console.log(event.target)
//   let shiftX = event.clientX - ball.getBoundingClientRect().left;
//   let shiftY = event.clientY - ball.getBoundingClientRect().top;

//   ball.style.position = 'absolute';
//   ball.style.zIndex = 10;
//   document.body.append(ball);

//   moveAt(event.pageX, event.pageY);

//   // переносит мяч на координаты (pageX, pageY),
//   // дополнительно учитывая изначальный сдвиг относительно указателя мыши
//   function moveAt(pageX, pageY) {
//     ball.style.left = pageX - shiftX + 'px';
//     ball.style.top = pageY - shiftY + 'px';
//   }

//   function onMouseMove(event) {
//     moveAt(event.pageX, event.pageY);
//   }

//   // передвигаем мяч при событии mousemove
//   document.addEventListener('mousemove', onMouseMove);

// // отпустить мяч, удалить ненужные обработчики
//   ball.onmouseup = function() {
//     document.removeEventListener('mousemove', onMouseMove);
//     ball.onmouseup = null;
//   };

};

ball.ondragstart = function() {
return false;
};


document.addEventListener('click', ((e) => {
  if (e.target === ball) {
   e.target.classList.add('active') 
  } else {
    ball.classList.remove('active');
    ball.querySelector('.handles').classList.remove('block-active')
  }
}) )


const handles = document.querySelectorAll('.handle')
handles.forEach((handle) => {
  const currentResizer = handle
  let pos = ball.getBoundingClientRect();
  currentResizer.addEventListener('mousedown', function(e) {
    // e.preventDefault()
    // console.log(e.target)
  
    window.addEventListener('mousemove',  resize)
    window.addEventListener('mouseup',  stopResize)
  })
  // return currentResizer

  function resize(e) {
  // console.log('eeeeeee',currentResizer)
  if (currentResizer.classList.contains('mr')) {
    // console.log(e.clientX, pos.left)
    ball.style.width = e.clientX - pos.left + 'px';
  }
}

function stopResize() {
  console.log('езда рулям')
  window.removeEventListener('mousemove',  resize)
}
})

})




//////////////////////////////////////////////////

const element = ref({
  x: 20,
  y: 100,
  width: 100,
  height: 100,
  isActive: false,
})


</script>

<template>
    <section class="desk">
      <div class="block block-move">
        <draggable-resizable-vue
          class="tasks-note"
          v-model:x="element.x"
          v-model:y="element.y"
          v-model:h="element.height"
          v-model:w="element.width"
          v-model:active="element.isActive"
        />
        <div class="tasks-note myNote" >
          <p>stay alive</p>
          <div class="handles">
            <div class="handle tl"></div>
            <div class="handle tc"></div>
            <div class="handle tr"></div>

            <div class="handle ml"></div>
            <div class="handle mr"></div>

            <div class="handle bl"></div>
            <div class="handle bc"></div>
            <div class="handle br"></div>
          </div>
        </div>
        
      </div>
    </section>
</template>

<style scoped>

.block {
  position: relative;
  background-color: #6B6B6B;
  padding: 20px 10px;
  border-radius: 10px;
}

.block-move {
  height: 100%;
  background-color: #6B6B6B;
  padding: 20px 10px;
  border-radius: 10px;
  grid-column: 3/5;
}


.desk {
  box-sizing: border-box;
  padding: 40px 60px;
  width: 100%;
  height: 85%;
  column-gap: 40px;
}

.tasks__list {
  margin: 0;
  padding: 0;
  list-style: none;

}

.tasks__item {
  transition: background-color 0.5s;
  margin-bottom: 10px;
  padding: 5px;
  text-align: center;
  border-radius: 10px;
  cursor: move;
  background-color: rgba(248, 225, 144, 1);

  transition: background-color 0.5s;
}

.tasks__item:last-child {
  margin-bottom: 0;
}

.tasks-note {
  resize: both;
  position: absolute;
  width: 80px;
  height: 80px;
  background-color: rgba(248, 225, 144, 1);
  margin-bottom: 10px;
  text-align: center;
  cursor: move; 
  padding: 5px;
}
.tasks-note:last-child {
  margin-bottom: 0;
}


.selected {
  opacity: 0.6;
}

.create-button {
  width: 30px;
  height: 30px;
  padding: 0;
  margin: 0;
  border: 0;
  background-color: rgba(248, 225, 144, 0.5);
position: absolute;
bottom: 10px;
right: 10px;
}

.create-button:hover {
 background-color: rgba(248, 225, 144, 1);
}



.active {
  border: 2px dotted white;
}

.handle {
  box-sizing: border-box;
  position: absolute;
width: 8px;
height: 8px;
border: 1px solid white;
}

.handles {
 display: none;
}

.block-active {
  display: block;
}

.tl {
  top: -10px;
  left: -10px;
}
.tc {
  top: -10px;
  left: 45%;
}
.tr {
  top: -10px;
  right: -10px;
}

.bl {
  bottom: -10px;
  left: -10px;
}
.bc {
  bottom: -10px;
  left: 45%;
}
.br {
  bottom: -10px;
  right: -10px;
}

.ml {
  top: calc(50% - 5px);
  left: -10px
}
.mr {
  top: calc(50% - 5px);
  right: -10px
}

</style>
