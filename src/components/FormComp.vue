<template>
  <v-app class="#E0E0E0">
     <v-main>
        <v-container fluid fill-height>
           <v-layout align-center justify-center>
              <v-flex xs12 sm8 md4>
                 <v-card class="elevation-12">
                    <v-toolbar dark color="gray" >
                       <v-toolbar-title>{{isRegister ? stateObj.register.name : stateObj.login.name}} </v-toolbar-title>
                    </v-toolbar>
                    <v-card-text>
                    <form ref="form" @submit.prevent="isRegister ? register() : login()">
                           <v-text-field
                             v-model="username"
                             name="username"
                             type="text"
                             placeholder="Unesite korisničko ime"
                             required
                          ></v-text-field>
                          
                           <v-text-field
                             v-model="password"
                             name="password"
                             type="password"
                             placeholder="Unesite lozinku"
                             required
                          ></v-text-field>

                          <v-text-field v-if="isRegister"
                             v-model="confirmPassword"
                             name="confirmPassword"
                             type="password"
                             placeholder="Ponovo unesite lozinku"
                             required
                          ></v-text-field>
                          <div class="red--text"> {{errorMessage}}</div>
                          <v-btn type="submit" class="mt-4" color="grey" value="log in">{{isRegister ? stateObj.register.name : stateObj.login.name}}</v-btn>
                          <div class="grey--text mt-4" v-on:click="isRegister = !isRegister;">
                             {{toggleMessage}}  
                          </div>
                     </form>
                    </v-card-text>
                 </v-card>
                 
              </v-flex>
           </v-layout>
        </v-container>
     </v-main>
  </v-app>
</template>

<script>
export default {
 name: "App",
 data() {
   return {
     username: "",
     password: "",
     confirmPassword: "",
     isRegister : false,
     errorMessage: "",
     stateObj: {
        register :{
           name: 'Registracija',
           message: 'Imate račun? Prijavite se.'
        },
        login : {
           name: 'Prijava',
           message: 'Registracija'
        }
     }
   };
 },
 methods: {
   login() {
     const { username } = this;
     this.$router.replace({ name: "dashboard", params: { username: username } });
     window.open("/", "_self");
     
   },
   register() {
      if(this.password == this.confirmPassword){
         this.isRegister = false;
         this.errorMessage = "";
         this.$refs.form.reset();
         window.open("/prijava", "_self");
      }
      else {
        this.errorMessage = "Netočna lozinka"
      }
   }
 },
     computed: {
      toggleMessage : function() { 
         return this.isRegister ? this.stateObj.register.message : this.stateObj.login.message }
   }
};
</script>

<style>
   template{
      background: "https://images.unsplash.com/photo-1629196613836-0a7e2541990a?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=834&q=80" ;
   }

</style>