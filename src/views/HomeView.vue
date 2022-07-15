<script>

export default {
  name: 'Home',
  data() {
      return{
          nonConnecter:true,
          connecter:false,
          mode:'login',
          nom:'',
          prenom: '',
          email:'',
          password:'',
         
      }
    },
    computed: {
      validateFields: function() {
        if(this.mode == 'create')
        {
            if(this.nom !='' && this.prenom !='' && this.email !='' && this.password !=''){
              return true;
            }else{
              return false;
            }
        }else
        {
          if(this.email !='' && this.password !='')
          {
            return true;
          }else{
            return false;
          }
        }
        
      },
      

    },
    methods: {
      switchToCreateAccount() {
        this.mode = 'create'
      },
      switchToLogin() {
        this.mode = 'login'
      },
     
      login() {
       this.$store.dispatch('login',{
        
          
          email:this.email,
          password:this.password
        })
        .then((response)=>{
          console.log(response)
           
            this.$router.push('/acceuil')
            
          
        })
        .catch((error)=>{
         console.log(error)
        })
        
      },
      createAccount() {
       this.$store.dispatch('createAccount',{
          nom:this.nom,
          prenom:this.prenom,
          email:this.email,
          password:this.password
        })
        .then((response)=>{
          console.log(response)
          this.mode = 'login';
        })
        .catch((error)=>{
          console.log(error)
        })
        
      },
    },
        
   
     
    
    mounted() {
     
       
        
       }
     
      
     
   
}
</script>

<template>
 <div class="min-h-full flex items-center justify-center py-12 px-4 sm:px-6 lg:px-8" v-if="nonConnecter">
  <div class="max-w-md w-full space-y-8 block p-6 rounded-lg shadow-lg bg-white max-w-md border-4 border-gray-400">
    <div>
      <!-- <img class="mx-auto h-11 w-auto" src="./assets/img/nazi.png" alt="Workflow"> -->
      <h2 class="mt-6 text-center text-3xl font-bold text-gray-900 underline" v-if="mode =='login'">CONNEXION</h2>
      <h2 class="mt-6 text-center text-3xl font-bold text-gray-900 underline" v-else>INSCRIPTION</h2>
      <p class="mt-2 text-center text-sm text-gray-600" v-if="mode == 'login'">
        Tu n'a pas de compte ?
        <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500" @click="switchToCreateAccount()"> s'inscrire </a>
      </p>
      <p class="mt-2 text-center text-sm text-gray-600" v-else>
        Tu as deja un compte ?
        <a href="#" class="font-medium text-indigo-600 hover:text-indigo-500" @click="switchToLogin()">se connecter</a>
      </p>
    </div>
    
    <form>
      <div class="grid grid-cols-2 gap-4"  v-if="mode =='create'">
        <div class="form-group mb-6">
          <input type="text" class="form-control
            block
            w-full
            px-3
            py-1.5
            text-base
            font-normal
            text-gray-700
            bg-white bg-clip-padding
            border border-solid border-gray-300
            rounded
            transition
            ease-in-out
            m-0
            focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="exampleInput123"
            aria-describedby="emailHelp123" placeholder="nom" v-model="nom">
        </div>
        <div class="form-group mb-6">
          <input type="text" class="form-control
            block
            w-full
            px-3
            py-1.5
            text-base
            font-normal
            text-gray-700
            bg-white bg-clip-padding
            border border-solid border-gray-300
            rounded
            transition
            ease-in-out
            m-0
            focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="exampleInput124"
            aria-describedby="emailHelp124" placeholder="prenom" v-model="prenom">
        </div>
      </div>
      <div class="form-group mb-6">
        <input type="email" class="form-control block
          w-full
          px-3
          py-1.5
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="exampleInput125"
          placeholder="Email address" v-model="email">
      </div>
      <div class="form-group mb-6">
        <input type="password" class="form-control block
          w-full
          px-3
          py-1.5
          text-base
          font-normal
          text-gray-700
          bg-white bg-clip-padding
          border border-solid border-gray-300
          rounded
          transition
          ease-in-out
          m-0
          focus:text-gray-700 focus:bg-white focus:border-blue-600 focus:outline-none" id="exampleInput126"
          placeholder="Password" v-model="password">
      </div>
       <div class="form-group form-check text-center mb-6" v-if=" mode == 'login' && status == 'error_loggin'">
        <p class="text-red-500 text-sm">
          Email ou mot de passe incorrect
        </p>
      </div> 
       <div class="form-group form-check text-center mb-6" v-if=" mode == 'create' && status == 'error_created'">
        <p class="text-red-500 text-sm">
         Cet email existe déja
        </p>
      </div> 
      <button type="button"  @click="login()" class="
        w-full
        px-6
        py-2.5
        bg-blue-600
        text-white
        font-medium
        text-xs
        leading-tight
        uppercase
        rounded
        shadow-md
        hover:bg-blue-700 hover:shadow-lg
        focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0
        active:bg-blue-800 active:shadow-lg
        disabled:bg-blue-200 disabled:shadow-lg 
        disabled:cursor-not-allowed
        transition
        duration-150
        ease-in-out" v-if="mode =='login'" :disabled="!validateFields" >
        <span v-if="status == 'loading'">
          
            <div class="spinner-border animate-spin inline-block w-8 h-8 border-4 rounded-full" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
         
        </span>
        <span v-else>Connexion</span>
        </button>

      <button type="button" class="
        w-full
        px-6
        py-2.5
        bg-blue-600
        text-white
        font-medium
        text-xs
        leading-tight
        uppercase
        rounded
        shadow-md
        hover:bg-blue-700 hover:shadow-lg
        focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0
        active:bg-blue-800 active:shadow-lg
        disabled:bg-blue-200 disabled:shadow-lg
        disabled:cursor-not-allowed
        transition
        duration-150
        ease-in-out" v-else :disabled="!validateFields" @click="createAccount()" >
          <span v-if="status == 'loading'">
          
            <div class="spinner-border animate-spin inline-block w-8 h-8 border-4 rounded-full" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
         
          </span>
          <span v-else>INSCRIPTION</span>
        </button>
    </form>

  </div>
</div>

  
</template>
