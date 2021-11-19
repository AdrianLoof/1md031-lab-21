<template>
  <header>
    <div class="container">
      <h1 id="headertext">Välkommen till Addes Burgare</h1>
      <img id="background" src="paddys.jpg">
    </div>
  </header>

  <main>
    <section id="testid">
      <h2>Välj Burgare</h2>
      <p>This is where you select burger</p>
      <div class="wrapper">
        <Burger class="box" v-for="burger in burgers"
                v-bind:burger="burger"
                v-bind:key="burger.name"
                v-on:orderedBurger="addToOrder($event)"/>
      </div>
    </section>

    <section id="testid2">
      <h3 class="movetitles">Customer Information</h3>
      <form>
        <p>
          <input type="radio" id="male" v-model="gender" value="male">
          <label for="male">Male</label>
        </p>

        <p>
          <input type="radio" id="female" v-model="gender" value="female">
          <label for="female">Female</label>
        </p>

        <p>
          <input type="radio" id="Other" v-model="gender" value="Other" checked>
          <label for="Other">Other</label>

        </p>


        <h4 class="movetitles">Delivery Information</h4>


        <p>
          <label for="Fullname">Full Name</label><br>
          <input type="text" v-model="fn" id="fullname" required="required" placeholder="Full Name">
        </p>

        <p>
          <label for="email">Email</label><br>
          <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">

        </p>


        <p>
          <label for="recipient">Recipient</label><br>
          <select id="recipient" v-model="rcp">
            <option selected="selected">Credit Card</option>
            <option>Delivery Payment</option>
            <option>Bitcoin</option>
            <option>Invoice</option>
            <option>Sing a song</option>
          </select>

        </p>
        <p>
          <label for="other">Other Information</label><br>
          <textarea id="other" v-model="txtarea"></textarea>
        </p>
      </form>

    </section>
    <button v-on:click="MarkDone()" class="button" style="margin-left: 20px  " type="submit">
      Make Order!
    </button>

  </main>
  <footer>
    <hr>
    Please do not sue me
  </footer>
  <input type="text" v-model="yourVariable">
  <div>
    {{ yourVariable }}
  </div>
  <div id="map">
    <div id="dots" v-on:click="setLocation">
      <div v-bind:style="{ left: location.x + 'px',
                      top: location.y + 'px'  }">
        T
      </div>
    </div>
  </div>
</template>

<script>
import Burger from '../components/Burger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io();
/*
function MenuItem(Name, calories, picture, gluten,milk) {
  this.name = Name; // The *this* keyword refers to the object itself
  this.ingredient =gluten ;
  this.lactose=milk;
  this.img = picture;
  this.kCal=calories;
  return this;
}

const BurgerArray=[
  new MenuItem("Addes",1200,"https://images.unsplash.com/photo-1613277367862-f8ef14db7748?ixid=MnwxMjA3fDB8MHxzZWFyY2h8MXx8YmlnJTIwYnVyZ2VyfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&w=1000&q=80",false,true),
  new MenuItem( "Robbans", 400,"https://www.schaer.com/sites/default/files/2000_Fiery%20Mexican%20Chicken%20Burger.jpg", true,  true ),
  new MenuItem("Elibonks", 300,"https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRP6JTkxNoHt9VElHKqalvT_8tVus0wQE61JA&usqp=CAU", false,  false)
];
console.log(BurgerArray);
*/
const BurgerArray = menu;
export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      yourVariable: "välj en burgare",
      burgers: BurgerArray,
      fn: "",
      em: "",
      gender: "",
      rcp: "",
      txtarea: "",
      orderedBurgers: {},
      location: {
        x: 0,
        y: 0
      }


      /*[ {name: "small burger", kCal: 250},
             {name: "standard burger", kCal: 450},
             {name: "large burger", kCal: 850}
           ] */
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random() * 100000);

    },
    setLocation:function(event){
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y

      }
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
    MarkDone: function () {
      console.log(this.gender, this.fn, this.em, this.rcp, this.txtarea, this.orderedBurgers);
      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: this.location.x,
              y: this.location.y
            },
            orderItems: [this.orderedBurgers]
          }
      );
    },
   /* addOrder: function (event) {

      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };


      socket.emit("addOrder", {
            orderId: this.getOrderNumber(),
            details: {
              x: event.clientX - 10 - offset.x,
              y: event.clientY - 10 - offset.y
            },
            orderItems: ["Beans", "Curry"]
          }
      );
      this.location = {
        x: event.clientX - 10 - offset.x,
        y: event.clientY - 10 - offset.y

    }*/
  }
}
</script>

<style>
@import 'https://fonts.googleapis.com/css?family=Pacifico|Dosis';

body {
  font-family: Arial, sans-serif;
  font-size: 15pt;
}

section {
  margin: 20px;
}

section div {
  padding: 60px;
}

.ingrediens {
  color: #ff5500;
}

.movetitles {
  margin-left: 10px;
}

#testid {
  text-transform: uppercase;
  background-color: black;
  color: white;
  border: dotted;

}

#testid2 {
  background-color: white;
  color: black;
  border: dotted #111010;


}

form p {
  padding-left: 30px;
}

.button:hover {
  cursor: pointer;
  background-color: chartreuse;
}

.container {
  position: relative;
  text-align: center;
  color: white;
  margin: 20px;
  border: groove #ff5500;
  overflow: hidden;
}

#background {
  align-content: center;
  width: 100%;
  height: 13em;
  opacity: 70%;
}

#headertext {
  position: absolute;
  top: 30%;
  left: 50%;
  z-index: 1;
  transform: translate(-50%, 50%);
}

.wrapper {
  /*display: grid;
  grid-gap: 40px;
 padding:100px; */

  background-color: #726d6d;
  color: #111010;
  display: grid;
  grid-gap: 250px;
  grid-template-columns: 100px 100px 100px;


}

.box {
  /* background-color: #444;
   color: #fff;
   border-radius: 50px;
   padding: 50px;
    font-size: 150%; */
}


.a {
  grid-column: 1;

}

.b {
  grid-column: 2;

}

.c {
  grid-column: 3;

}


#dots {
  position: relative;
  margin: 0;
  padding: 0;
  background: url(/img/polacks.jpg);
  background-repeat: no-repeat;
  width: 1920px;
  height: 1078px;
  cursor: crosshair;

}

#map {
  width: 400px;
  height: 500px;
  overflow: scroll;
  margin-left: 200px;

}

#dots div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width: 20px;
  height: 20px;
  text-align: center;
}

</style>
