<template>

<div class="main_class">

    <div id='cssmenu' >
<ul>

 
    <li >
 
      <router-link :to="{ name: 'HomeAdmin', params: { user: user , type: type }}">
 
      <a><span>Home</span></a>
 
      </router-link>
 
      </li>
 

 
   <li >
 
       <router-link :to="{ name: 'MenuPrograma', params: { user: user , type: type}  }">
 
       <a ><span>Editar Programa</span></a>
 
       </router-link>
 
       </li>
 
        <li>
 
        <router-link :to="{ name: 'PlanEstudio', params: { user: user , type: type}  }">
 
        <a><span>Crear Plan de Estudio</span></a>
 
        </router-link>
 
        </li>
 
   <li class='active has-sub'>
 
       <router-link :to="{ name: 'BuscarPorFechaAdmin', params: { user: user , type: type }}">
 
         <a><span>Buscar por Fecha/Asignatura</span></a>
 
        </router-link>
 
       </li>
    

</ul>

</div>

<div class="content-box-top">Bienvenido al portal.<a href="#/"><span class="cerrar">Cerrar sesión</span></a></div>
<div class="content-box-left"><img src="../assets/usach.png" height="100" width="200"></div>
<div class="content-box-down">Abajo.</div>


<div>
    <h4 class="titulo">1. PROGRAMA DE LA ASIGNATURA </h4>
    <table class="busqueda">
        <td>
            <th>
            <input type="radio" name="rad" value="1" v-on:click="actualizaValor(1)" checked> Buscar por fecha
            </th>
            <tr>Ingrese Fecha   :
            </tr>
            <tr >Ingrese  Asignatura    :
            </tr>
        </td>
        <td>
            <th>
            </th>
            <tr contenteditable="true"> 
            
            <input  ref="fechaProg" class="texto_input" type="date" name="fecha" step="1" min="2018-01-01" max="2030-12-31" value="2018-05-10">
            
            </tr>
            <tr>
                <!-- 
                    con v-for se itera sobre la varible Asignaturas y se guardan los valores en asignatura
                    Se obtienen los datos de asignatura
                -->
                <select  v-for="asignatura in Asignaturas" id="selectAsig">
                        <option :value="asignatura.nombreAsignatura">{{asignatura.nombreAsignatura}}</option>
                        
                </select>
            </tr>
        
        </td>
        <td>
            <th>
                <input type="radio" name="rad" value="2" v-on:click="actualizaValor(2)">Buscar por código
            </th>
            <tr>
                Ingrese Código : <input type="text"  style="width:80px;" ref="codProg"/>
            </tr>
            
            <tr >
                <br/>
                <input type="button" id="boton" class="botones" value="Buscar" v-on:click="buscar" />
            </tr>
        </td>
         <td>
            <th>
             <input type="radio" name="rad" value="3" v-on:click="actualizaValor(3)">Buscar por nivel
            </th>
             <tr>
                Ingrese nivel: <input type="text"  style="width:60px;" ref="nivelProg"/>
            </tr>
            <tr>
                
            </tr>
        </td>
        <td>
            
            
           
            
        </td>
        

    </table>
    <div class="resulta">
        <h4 >RESULTADOS </h4>
       
        <ul v-for="program in programs">
             <table class="tabla_res">
           
            <td>
                <tr class="desaparece">.</tr>
                <tr>Nombre :</tr>
                <tr>Asignatura :</tr>
                <tr>Departamento :</tr>
                <tr>Estado Programa</tr>
                <tr>Fecha última modificación :</tr>
            </td>
            <td>
                <tr class="desaparece">.</tr>
                <tr>{{program.nameprograma}}</tr>
                <tr>{{program.asignprograma}}</tr>
                <tr>{{program.deptoprograma}}</tr>
                <tr>{{program.estadoprograma}}</tr>
                <tr>{{program.fechaultmod}}</tr>
            </td>
            <td class="desaparece">
                
              <tr class="desaparece">......................</tr>
            </td>
            <td><tr class="desaparece"><a href='#'><img src="../assets/des.png" height="40" width="40"></a>..</tr></td>
            <td><tr class="desaparece"><a href='#'><img src="../assets/asig.png" height="40" width="40"></a>..</tr></td>
            <td><tr><a href="/" ><img src="../assets/edit.png" height="40" width="40"></a></tr>
                <tr class="desaparece">.</tr>
                <tr class="desaparece">.</tr>
                <tr class="desaparece">.</tr>
               
                 <tr class="desaparece">.</tr>
                <tr class="desaparece">.</tr>
                <tr class="ver_detalle"><a href="#">Ver detalle ...</a></tr>
            </td>
            <td>
                 <tr class="desaparece">.</tr>
               
            </td>

           
            
        </table>
        <br>
        <br>
        </ul>
        


    </div>
</div>

</div>
</template>

<script>
var valor = 1;


export default {
    // Variables que pasan desde otros archivos
    props: ['user','type'],
    methods: {
    edicion: function(event) {
      var id_prog = 1;
    },
    actualizaValor: function(oRad){
        valor = oRad;
   },

    /*  Funcion que realiza la busqueda de programas dependiendo del valor del radio seleccionado
        retorna el resultado de la busqueda y lo guarda en la variable programs
    */
     buscar: function(){
         //obtiene valor del radio
        var val = valor;
        if (val==1){
            // busqueda por fecha y nombre asginatura
            var combo = document.getElementById('selectAsig');
            var fechaProg = this.$refs.fechaProg.value;
            var nomAsig = combo.options[combo.selectedIndex].value
        this.$http.get('http://localhost:5000/programNF?asignatura='+nomAsig+'&fecha='+fechaProg)
        .then(response =>{
            this.programs = response.body.program
            }, response =>{
                console.log("falla ")
            });
        }
        else if(val==2){
            // busqueda por codigo de asignatura
            var codigo = this.$refs.codProg.value;
            this.$http.get('http://localhost:5000/programCodigo?codigo='+codigo)
        .then(response =>{
            this.programs = response.body.program
            console.log(this.programs)
            }, response =>{
                console.log("falle :c ")
            });

        }
        else{
            //busqueda por nivel
            console.log("entramos a busqueda 3 "+val);
             var nivel = this.$refs.nivelProg.value;
             this.$http.get('http://localhost:5000/programNivel?nivel='+nivel)
        .then(response =>{
            this.programs = response.body.program
            console.log(this.programs)
            }, response =>{
                console.log("falle :c ")
            });
        }
       
    }
    },

    
    /*  Funcion que se ejecuta antes de cargar la pagina
    */ 
     mounted: function(){
         //obtiene las asignaturas que luego se pueden seleccionar en la busqueda
       this.$http.get('http://localhost:5000/asignaturas')
      .then(response =>{
        this.Asignaturas = response.body.asignaturas
        console.log(this.Asignaturas)
      }, response =>{
        console.log("falle :c ")
      });
       console.log(this.type)
       if(this.type == "Admin"){
           
        this.isAdmin=true;
 
      }
 
    },

    /*  Aqui se definen las variables utilizadas  
     */
    data() {

        return {
            
             Asignaturas: [],
             programs :[],
             isAdmin: true
           

        }
    
    }
    

}
</script>

<style>

.main_class {
     background:#e6e3e3;
     
}
.busqueda {
    position: absolute; top: 120px; left: 300px;
    
    background:#cdd1f8cc;
    
}

.desaparece {
    color: #e6e3e3;
}
.tabla_res{
    
    margin: 20px;
    border:1px solid #ea750067;
}

.ver_detalle{
    color: #ea7600;
    font-size: 18px;
    font-style: inherit;
}
.resulta {
    position: absolute; top: 240px; left: 300px;
    width: 900px;
    background:#cdd1f8cc;
}

.titulo {
    position: absolute; top: 60px; left: 250px;
}

.texto_input{
  background-color: #ffffffc0;
}

.botones {
    background: #ea7600;
        border: none;
        
        color: #eee;
        
        font-size: 15px;
        margin: 5px 0;
        padding: 5px 22px;
        -moz-border-radius: 4px;
        border-radius: 4px;
        -webkit-border-radius:4px;
        
}
.content-box-top {
margin: 0 0 25px;
overflow: hidden;
padding: 21px;
color: #b1b1b1;
}

.content-box-top {
background-color: #002f6c;
position:absolute; top:0px; left: 0px ; right: 0px;
}

.cerrar {
  position: absolute; right: 80px;
  color: #ea7600;
}
.content-box-left {
background-color: #b1b1b1;
position:absolute; top:59px; left: 0px ;
height: 45em;
width: 13em;
}




@import '../fondo.css';


</style>
