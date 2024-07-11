<script setup>
import {onMounted, ref} from 'vue'

onMounted(() => {
const activeNote = ref(null)

const moveBlock = document.querySelector('.block-move')
const notes = document.querySelectorAll('.myNote')
let position = null

notes.forEach((ball) => {

ball.onmousedown = function(event) {
  if (activeNote.value !== event.target.id) {
    if (activeNote.value !== null) {
      position = null
      activeNote.value.classList.remove('active');
      activeNote.value.querySelector('.handles').classList.remove('block-active')
    }
    
    activeNote.value = event.target
  }

  activeNote.value.classList.add('active')
  activeNote.value.querySelector('.handles').classList.add('block-active')
  let shiftX = event.clientX - activeNote.value.getBoundingClientRect().left;
  let shiftY = event.clientY - activeNote.value.getBoundingClientRect().top;

  activeNote.value.style.position = 'absolute';
  activeNote.value.style.zIndex = 10;
  document.body.append(activeNote.value);

  moveAt(event.pageX, event.pageY);

  // переносит мяч на координаты (pageX, pageY),
  // дополнительно учитывая изначальный сдвиг относительно указателя мыши
  function moveAt(pageX, pageY) {
    activeNote.value.style.left = pageX - shiftX + 'px';
    activeNote.value.style.top = pageY - shiftY + 'px';
    position = activeNote.value.getBoundingClientRect();
  }

  function onMouseMove(event) {
    moveAt(event.pageX, event.pageY);
  }

  // передвигаем мяч при событии mousemove
  moveBlock.addEventListener('mousemove', onMouseMove);

// отпустить мяч, удалить ненужные обработчики
activeNote.value.onmouseup = function() {
    moveBlock.removeEventListener('mousemove', onMouseMove);
    activeNote.value.onmouseup = null;
  };

};

ball.ondragstart = function() {
return false;
};

document.addEventListener('click', ((e) => {
  if (e.target !== ball && activeNote.value !== null) {
    activeNote.value.classList.remove('active');
    activeNote.value.querySelector('.handles').classList.remove('block-active')
  }
}) )


const handles = document.querySelectorAll('.handle')
handles.forEach((handle) => {
  const currentResizer = handle
  currentResizer.addEventListener('mousedown', function(e) {
    position = activeNote.value.getBoundingClientRect();
    // console.log("положение", e.clientX, "отступ блока слева", position.left, "ширина блока", position.width)

    window.addEventListener('mousemove',  resize)
    window.addEventListener('mouseup',  stopResize)
    e.stopPropagation()
  })


  function resize(e) {
  // боковые
  if (currentResizer.classList.contains('mr')) {
    // console.log(e.clientX, position.left)
    activeNote.value.style.width =  e.clientX - position.left - 8 + 'px';
  }
  if (currentResizer.classList.contains('ml')) {
    // console.log(e.clientX, position.left)
    activeNote.value.style.left = e.clientX + 'px'
    activeNote.value.style.width = position.width + (position.left - e.clientX )  + 'px'
  }

  // вертикальные
  if (currentResizer.classList.contains('bc')) {
    // console.log(e.clientX, pos.left)
    activeNote.value.style.height = e.clientY - position.top - 8 + 'px';
  }
  if (currentResizer.classList.contains('tc')) {
    activeNote.value.style.top = e.clientY + 'px'
    activeNote.value.style.height = position.height + (position.top - e.clientY - 8) + 'px'
  }

// угловые
  if (currentResizer.classList.contains('br')) {
    if (e.clientX - position.left - 8 >= minSize.value.width && e.clientY - position.top - 8 >= minSize.value.height) { 
      activeNote.value.style.width = e.clientX - position.left - 8 + 'px';
      activeNote.value.style.height = e.clientY - position.top - 8 + 'px';
    }
  }
  if (currentResizer.classList.contains('bl')) {
    if (position.width + (position.left - e.clientX) >= minSize.value.width && e.clientY - position.top - 8 >= minSize.value.height) {
      activeNote.value.style.left = e.clientX + 'px'
      activeNote.value.style.width = position.width + (position.left - e.clientX )  + 'px'
      activeNote.value.style.height = e.clientY - position.top - 8 + 'px';
    }
  }

  if (currentResizer.classList.contains('tr')) {
    if (e.clientX - position.left - 8 >= minSize.value.width && position.height + (position.top - e.clientY - 8) >= minSize.value.height) {
      activeNote.value.style.width = e.clientX - position.left - 8 + 'px';
      activeNote.value.style.top = e.clientY + 'px'
      activeNote.value.style.height = position.height + (position.top - e.clientY - 8) + 'px' 
    }
  }
  if (currentResizer.classList.contains('tl')) {
    if (position.width + (position.left - e.clientX) >= minSize.value.width && position.height + (position.top - e.clientY - 8) >= minSize.value.height) {
      activeNote.value.style.left = e.clientX + 'px'
      activeNote.value.style.width = position.width + (position.left - e.clientX) + 'px'
      activeNote.value.style.top = e.clientY + 'px'
      activeNote.value.style.height = position.height + (position.top - e.clientY - 8) + 'px'
    }
  }
}

  function stopResize() {
    window.removeEventListener('mousemove',  resize)
  }
})

})
})
//////////////////////////////////////////////////

const minSize = ref({
  width: 20,
  height: 20,
  isActive: false,
})


</script>

<template>
    <section class="desk">
      <div class="block block-move">
        <div class="tasks-note myNote"  
          :class="{'active' : activeNote === 1}"
          id="1"
        >
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
        
        <div class="tasks-note myNote" id="2" 
          style="  background-color: rgb(248, 168, 144); top: 120px"
        >
          <p>А?</p>
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

        <div class="tasks-note myNote" id="2" 
          style="background-color: rgb(191, 248, 144);   top: 220px"
        >
          <p>nani?</p>
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
