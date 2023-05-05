<template>
    <div> 
      <div class="row">
        <div class="col"></div><strong>the best</strong>
        <div class="col"><h3 class="text-center">Crud Vue Jhon Ariza</h3>  <img  src="img/vuee.png"    srcset=""> </div>
        <div class="col"></div>
      </div>
     
      <!-- Button to Open the Modal -->
      <!-- se agreaga boton con el evento click   llama al metodo abrir modal -->
  <div class="bg-primary"><button @click="modificar=false; abrirModal();" type="button"    class="btn btn-outline-warning " style="margin-top: 10px;margin-left:350px;">Nuevo Articullo</button>
    <hr />
</div>
      <!-- The Modal -->
      <!-- se usa la directiva vue la cual se aplica mostrar cuando modal sea verdadero  -->
      <div class="modal" :class="{mostrar: modal}">
        <div class="modal-dialog">
          <div class="modal-content">
            <!-- Modal Header -->
            <div class="modal-header">
              <h4 class="modal-title">{{tituloModal}}</h4>
              <!-- SE LLAMA AL metodo cerrar modal -->
              <button @click="cerrarModal();"  type="button" class="close" data-dismiss="modal">
                &times;
              </button>
            </div>
  
            <!-- Modal body -->
            <div class="modal-body">
              <div class="my-4">
                <label for="nombre">Nombre</label>
                <input v-model="articulo.nombre" type="text" class="form-control" id="nombre" placeholder="Nombre del Articulo">
              </div>
              <div class="my-4">
                <label for="descripcion">descripcion</label>
                <input v-model="articulo.descripcion" type="text" class="form-control" id="descripcion" placeholder="Descripcion del Articulo">
              </div>
              <div class="my-4">
                <label for="stock">stock</label>
                <input v-model="articulo.stock" type="number" class="form-control" id="stock" placeholder="Stock del Articulo">
              </div>
            </div>
  
            <!-- Modal footer -->
            <div class="modal-footer">
              <button @click="cerrarModal();" type="button" class="btn btn-secondary" data-dismiss="modal">
                Cancelar
              </button>
              <!-- se llama al metodo guardar -->
              <button  @click="guardar();" type="button" class="btn btn-success" data-dismiss="modal">
                Guardar
              </button>
  
  
            </div>
          </div>
        </div>
      </div>
  
      <table  class="table table-dark table-striped">
        <thead class="thead-dark">
          <tr>
            <th scope="col">#</th>
            <th scope="col">Nombre</th>
            <th scope="col">Descripcion</th>
            <th scope="col">Stock</th>
            <th scope="col" colspan="2" class="text-center">Accion</th>
          </tr>
        </thead>
        <tbody>
          <!-- se recorre el array articulos -->
          <tr v-for="art in articulos" :key="art.id">
            <th scope="row">{{ art.id }}</th>
            <td>{{ art.nombre }}</td>
            <td>{{ art.descripcion }}</td>
            <td>{{ art.stock }}</td>
            <td>
            <!--   -->
              <button  @click="modificar=true; abrirModal(art);" class=" btn btn-outline-primary">Editar</button>
            </td>
            <td>
              <!-- en el evento click se llama al metodo eliminar pasandole el id para que este disponible -->
              <button @click="eliminar(art.id)" class="btn btn-outline-danger">
                Eliminar
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    // se crea una data 
    // retorna un array  de todos los articulos  para poderlo recorrer  
    //primero se nesecita un metodo 
    data() {
      return {
        // se crea el objeto que contiene esos campos 
        articulo:{
          nombre:'new',
          descripcion:'crazy',
          stock:1,
        },

        // se crean las variables que se usan
        // el id por defecto es cero  en caso que sea para editar va a ser 1 
        id:0,
        // se crea variable mofificar y se iniciliza en true
        modificar:true,
        // esta modal es cero por que indica que el modal va a estar cerrado
        modal:0,
        tituloModal:'',
        articulos: [],
      };
    },
    methods: {
     
      
      async listar() {
 
   // se accede ala ruta index  de los articulos atrvez de axios 
       const res = await axios.get('/articulos'); 
        // this es para poder acceder alos elementos  
        // y sele pasa la respuesta ala data
        this.articulos = res.data;
      },
      async buscar(){
        const res = await axios.get('/articulos'); 
      },
      // se accede al metodo eliminar  atravez de la ruta de articulos
      // atravez del id 
      async eliminar(id) {
        const res = await axios.delete('/articulos/' + id);
        // una vez que elimine se vuelve a cargar todo el array de articulos
        this.listar();
      },
      // en este metodo 
      async guardar() {
        if(this.modificar){
          //se modifica atraves del verbo put 
          const res = await axios.put('/articulos/'+this.id, this.articulo);
          // console.log(this.id);
        }else{
          const res = await axios.post('/articulos/', this.articulo);
        }
        this.cerrarModal();
        this.listar();
      },

      // este metodo pone el modal en 1 
      // se le pasa todo el objeto completo
      //
      abrirModal(data={}){
        this.modal=1;
        // si modificar true va a poner estos campos 
        if(this.modificar){
          //
          this.id=data.id;
          this.tituloModal="Modificar Articulo";
          this.articulo.nombre=data.nombre;
          this.articulo.descripcion=data.descripcion;
          this.articulo.stock=data.stock;
        }else{
          this.id=0;
          this.tituloModal="Crear Articulo";
          this.articulo.nombre='';
          this.articulo.descripcion='';
          this.articulo.stock=1;
        }
      },
      // 
      cerrarModal(){
        this.modal=0;
      },
    },
    // se crea un ciclo para que cuando se va a crear el metodo listar llame todos los registros
    created() {
      this.listar();
    },
  };
  </script>
  
  <style>
  /* para abrir el modal de crear  */
    .mostrar{
      display: list-item;
      opacity: 1;
      background: rgba(75, 56, 143, 0.705);
    }
  </style>