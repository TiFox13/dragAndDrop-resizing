<script setup>
import {onMounted, ref} from 'vue'
// import VueResizable from 'vue-resizable'
import DraggableResizableVue from 'draggable-resizable-vue3'

onMounted(() => {
  const moveBlock = document.querySelector('.block-move')
  const note = moveBlock.querySelector('.tasks-note')
  let myNote = null
    moveBlock.addEventListener(`dragstart`, (e) => {
      e.target.classList.add(`selected`);
      myNote = e.target
    });

    moveBlock.addEventListener(`dragend`, (e) => {
      e.target.classList.remove(`selected`);
    });

    moveBlock.addEventListener(`drag`, (e) => {

    })

    moveBlock.addEventListener(`dragover`, (e) => {
      e.preventDefault();

    moveBlock.addEventListener(`drop`, (e) => {
      myNote.style.left = `${e.clientX - myNote.offsetWidth / 2}px`
      myNote.style.top = `${e.clientY - myNote.offsetHeight / 2}px`
    });
  })

//   function direction( elem, e ) {
//     let res = 8;
//     let pad = 4;
//     // let pad = pad || 4;
//     let pos = e.target.getBoundingClientRect();
//     let top = pos.top;
//     let left = pos.left;
//     let width = e.target.clientWidth;
//     let height = e.target.clientHeight;
//     let eTop = e.clientY;
//     let eLeft = e.clientX;
//     var isTop = eTop - top < pad;
    
//   let isRight = left + width - eLeft < pad;
//   let isBottom = top + height - eTop < pad;
//   let isLeft = eLeft - left < pad;
//   if ( isTop ) res = 0;
//   if ( isRight ) res = 1;
//   if ( isBottom ) res = 2;
//   if ( isLeft ) res = 3;

// // Это ещё не всё — надо также учесть ресайз по диагонали:
//   if ( isTop && isRight ) res = 4;
//   if ( isRight && isBottom ) res = 5;
//   if ( isBottom && isLeft ) res = 6;
//   if ( isLeft && isTop ) res = 7;
//   return res;		// если ни одно из условий не сработает,
// 			// то res так и останется 8
//   }

//   var cursors = "n w s e ne se sw nw".split(" ");

//   // function Resizable( elem, options ) {
// 	// options = options || {};
// 	// options.max = options.max || [1E17, 1E17];
// 	// options.min = options.min || [10, 10];
// 	// options.allow = (options.allow || "11111111").split("");

// // elem.min = options.min;
// // elem.max = options.max;
// // elem.allow = options.allow;
// // elem.pos = elem.getBoundingClientRect();
//   // }

//   note.addEventListener( "mousemove", function ( e ) {
//     // console.log('kzkzkz')
// 	var dir = direction( this, e );
//   // console.log(dir)
// 	// if ( options.allow[dir] === "0" ) return;
// 	this.style.cursor = dir === 8 ? "default" : cursors[ dir ] + "-resize";
// 	// вот и пригодилась cursors
// } );

//   note.addEventListener( "mousedown", resizeStart );
// document.body.onselectstart = function (e) { return false };


//   function resizeStart( ev ) {
//     // console.log(this)
// 	var dir = direction( this, ev ); // вычисляем направление

//   // options = options || {};
// 	// options.max = options.max || [1E17, 1E17];
// 	// options.min = options.min || [10, 10];
// 	// options.allow = (options.allow || "11111111").split("");


// 	// if ( this.allow[dir] == "0" ) return; // если направление не разрешено, отменяем ресайз
// 	document.documentElement.style.cursor = this.style.cursor = cursors[ dir ] + "-resize";
// 	var pos = this.getBoundingClientRect();
// 	var elem = this; // для работы в mousemove документа
// 	var height = this.clientHeight;
// 	var width = this.clientWidth;
// 	// при каждом движении мыши на документе будет
// 	// срабатывать resize(). Её определение будет ниже.
// 	document.addEventListener( "mousemove", (e) => resize(e, dir) );
// 	// при отпускании кнопки мыши удаляем обработчик
// 	// и ставим стандартный курсор
// 	document.addEventListener( "mouseup", function () {
    
// 		document.removeEventListener( "mousemove", (e) => resize(e, dir) );
// 		document.documentElement.style.cursor = elem.style.cursor = "default";
// 		document.body.onselectstart = null;
// 	});


//   function resize ( e, dir ) {
// 	// при ресайзе вверх либо вверх и вправо либо вверх и влево
// 	// изменяем высоту и отступ сверху
// 	// все остальные if-ы работают так же
// 	if ( dir === 0 || dir === 4 || dir === 7 ) {
// 		// elem.style.top = e.clientY - ev.clientY + pos.top;
// 		// elem.style.height = height + ev.clientY - e.clientY;
// 	}
// 	if ( dir === 1 || dir === 4 || dir === 5 ) {
// 		// elem.style.width = e.clientX - pos.left;
// 	}
// 	if ( dir === 2 || dir === 5 || dir === 6 ) {
// 		// elem.style.height = e.clientY - pos.top;
// 	}
// 	if ( dir === 3 || dir === 6 || dir === 7 ) {
//     console.log(3)
// 		// elem.style.left = e.clientX - ev.clientX + pos.left;
// 		// elem.style.width = width + ev.clientX - e.clientX;
// 	}
// }

// }

////////////////////////////////////////////////////////

  let source = null
  let target = null
  let activeElement = null
  const tasksListElement = document.querySelectorAll('.block')

tasksListElement.forEach(element => {

  element.addEventListener(`dragstart`, (evt) => {
    source = evt.target.parentElement
    evt.target.classList.add(`selected`);
    activeElement = evt.target
}) 
  element.addEventListener(`dragend`, (evt) => {
  evt.target.classList.remove(`selected`);
  });

  element.addEventListener(`dragover`, (evt) => {
    evt.preventDefault();

  
  // Находим в каком из блоков сейчас находится перемещаемый подблок
  const target = element
  // Находим элемент, над которым в данный момент находится курсор
  const currentElement = evt.target;

  // Если мы не в родном блоке.
if (target.id !== source.id) {

  target.addEventListener("drop", (e) => {
    e.target.appendChild(activeElement);
});
} 

  // Проверяем, что событие сработало:
  // 1. не на том элементе, который мы перемещаем,
  // 2. именно на элементе списка
  const isMoveable = activeElement !== currentElement &&
    currentElement.classList.contains(`tasks__item`);

  // Если нет, прерываем выполнение функции
  if (!isMoveable) {
    return;
  }

  // evt.clientY — вертикальная координата курсора в момент,
  // когда сработало событие
  const nextElement = getNextElement(evt.clientY, currentElement);

  // Проверяем, нужно ли менять элементы местами
  if (
    nextElement &&
    activeElement === nextElement.previousElementSibling || activeElement === nextElement
  ) {
    // Если нет, выходим из функции, чтобы избежать лишних изменений в DOM
    return;
  }
    element.insertBefore(activeElement, nextElement);
});
});
});

const getNextElement = (cursorPosition, currentElement) => {
  // Получаем объект с размерами и координатами
  const currentElementCoord = currentElement.getBoundingClientRect();
  // Находим вертикальную координату центра текущего элемента
  const currentElementCenter = currentElementCoord.y + currentElementCoord.height / 3;

  // Если курсор выше центра элемента, возвращаем текущий элемент
  // В ином случае — следующий DOM-элемент
  const nextElement = (cursorPosition < currentElementCenter) ?
      currentElement :
      currentElement.nextElementSibling;

  return nextElement;
};
// })


const element = ref({
  x: 20,
  y: 20,
  width: 200,
  height: 200,
  isActive: false,
})

</script>

<template>
    <section class="desk">
      
      <div class="block" id="block1">
        <li class="tasks__item" style="grid" draggable="true">learn HTML</li>
        <li class="tasks__item" draggable="true">learn CSS</li>
        <li class="tasks__item" draggable="true">learn JavaScript</li>
        <li class="tasks__item" draggable="true">learn PHP</li>
        <li class="tasks__item" draggable="true">stay alive</li>
        <li class="tasks__item" draggable="true">learn HTML</li>
        <button class="create-button">+</button>
      </div>
       
      <div class="block" id="block2">
        <li class="tasks__item" draggable="true">stay alive</li>
        <li class="tasks__item" draggable="true">learn CSS</li>
        <li class="tasks__item" draggable="true">learn JavaScript</li>
        <li class="tasks__item" draggable="true">learn PHP</li>
      </div>

      <div class="block block-move" id="block3">
              <!-- <vue-resizable></vue-resizable> -->
        <draggable-resizable-vue
          class="tasks-note"
          v-model:x="element.x"
          v-model:y="element.y"
          v-model:h="element.height"
          v-model:w="element.width"
          v-model:active="element.isActive"
        />
        <li class="tasks-note" >stay alive</li>
        <button class="create-button">+</button>
      </div>
    </section>
</template>

<style scoped>

.block {
  position: relative;
  /* display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(10, 1fr);
  gap: 10px; */
  background-color: #6B6B6B;
  padding: 20px 10px;
  border-radius: 10px;
}

.block-move {
  /* position: relative; */
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
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  column-gap: 40px;
  /* border: 1px solid red; */
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
  /* width: 400px; */

  text-align: center;
  /* border: 2px dashed #b2d9d0; */
  border-radius: 10px;
  cursor: move;
  background-color: rgba(248, 225, 144, 1);

  transition: background-color 0.5s;
}

.tasks__item:last-child {
  margin-bottom: 0;
}

.tasks-note {
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

</style>
