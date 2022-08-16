<template>
  <div id="login-container">
    <div class="flex flex-wrap min-h-screen w-full content-center justify-center bg-gray-200 py-10">
      <div class="flex shadow-md content-center justify-center">
        <div class="flex flex-wrap content-center justify-center rounded-l-md bg-white">
          <div class="md:w-72 mx-6 my-6">
            <h1 class="text-xl font-semibold">Seja bem vindo!</h1>
            <small class="text-gray-400">Por favor insira seus dados de login</small><br>
            <small v-if="showLoginMensageAlert" class="text-red-500">Login não reconhecido!</small>
            <form class="mt-4">
              <div class="mb-3 text-start">
                <label class="mb-2 block text-xs font-semibold">Email</label>
                <input v-model="email" type="email" placeholder="Digite seu e-mail" class="block w-full rounded-md border border-gray-300 focus:border-purple-700 focus:outline-none focus:ring-1 focus:ring-purple-700 py-1 px-1.5 text-gray-500" />
              </div>
              <div class="mb-3 text-start">
                <label class="mb-2 block text-xs font-semibold">Senha</label>
                <input v-model="password" type="password" placeholder="*****" class="block w-full rounded-md border border-gray-300 focus:border-purple-700 focus:outline-none focus:ring-1 focus:ring-purple-700 py-1 px-1.5 text-gray-500" />
              </div>
              <div class="mb-3 content-center">
                <a v-on:click="onShowModalUpdate('recovery')" class="text-color text-xs font-semibold cursor-pointer">Esqueceu a senha?</a>
              </div>
              <div class="mb-3 content-center">
                <div class="flex">
                  <button v-on:click="onClickSignIn" class="btn-login mb-1.5 mr-2 block w-full text-center text-white px-2 py-1.5 rounded-md text-xs">Login</button>
                  <button v-on:click="onClickSignIn" class="btn-login--sso mb-1.5 block w-full text-center text-white px-2 py-1.5 rounded-md text-xs">Login com SSO</button>
                </div>
                <button class="btn-login--outline flex flex-wrap w-full justify-center border border-gray-300 px-2 py-1.5 rounded-md text-xs">
                  <img class="w-4 mr-2" src="https://lh3.googleusercontent.com/COxitqgJr1sJnIDe8-jiKhxDx1FrYbtRHKJ9z_hELisAlapwE9LUPh6fcXIfb5vwpbMl4xl9H9TRFPc5NOO8Sb3VSgIBrfRYvW6cUA">Login com Google</button>
              </div>
            </form>
            <div class="text-center">
              <span class="text-xs text-gray-400 font-semibold">Não tem conta?</span>
              <a v-on:click="onShowModalUpdate('signUp')" class="text-color text-xs font-semibold cursor-pointer"> Registre-se</a>
            </div>
          </div>
        </div>
        <div class="img-login md:flex hidden flex-wrap content-center justify-center rounded-r-md">
          <LoginImage />
        </div>
      </div>
    </div>

    <div v-if="showModalUpdate" class="flex transition ease-in-out delay-150 bg-black bg-opacity-50 absolute inset-0 justify-center items-center " id="overlay">
      <div class="transition ease-in-out delay-150 relative w-11/12 px-2 max-w-xs bg-white rounded-lg shadow dark:bg-gray-700">
        <button v-on:click="onCloseUpdatemodal" type="button" class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-800 dark:hover:text-white" data-modal-toggle="authentication-modal">
          <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
          <span class="sr-only">Close modal</span>
        </button>
        <div class="py-6 px-6 lg:px-8">
          <h3 class="mb-4 text-xl font-medium text-gray-900 dark:text-white">{{modalUpdateTitle}}</h3>
            <div v-if="ShowUpdateMensageAlert" class="mb-4 text-sm font-medium text-red-500">{{UpdateMensageAlert}}</div>
            <form class="space-y-6" action="#">
              <div class="mb-3 text-start">
                <label class="mb-2 block text-xs font-semibold">Email</label>
                <input v-model="UpdateEmail" type="email" placeholder="Digite seu e-mail" class="block w-full rounded-md border border-gray-300 focus:border-purple-700 focus:outline-none focus:ring-1 focus:ring-purple-700 py-1 px-1.5 text-gray-500" required/>
              </div>
              <div class="mb-3 text-start">
                <label class="mb-2 block text-xs font-semibold">{{modalUpdatePassword}}</label>
                <input  v-model="UpdatePassword" type="password" placeholder="*****" class="block w-full rounded-md border border-gray-300 focus:border-purple-700 focus:outline-none focus:ring-1 focus:ring-purple-700 py-1 px-1.5 text-gray-500" required/>
              </div>
              <div class="mb-3 text-start">
                <label class="mb-2 block text-xs font-semibold">Repita sua senha</label>
                <input v-model="UpdateConfirmPassword" type="password" placeholder="*****" class="block w-full rounded-md border border-gray-300 focus:border-purple-700 focus:outline-none focus:ring-1 focus:ring-purple-700 py-1 px-1.5 text-gray-500" required/>
              </div>
            <button v-on:click="onClickUpdateAccount" type="submit" class="w-full text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">{{modalUpdateButton}}</button>
          </form>
        </div>
      </div>
    </div>

    <div v-if="showModalSucess" class="flex transition ease-in-out delay-150 bg-black bg-opacity-50 absolute inset-0 justify-center items-center " id="overlay">
      <div class="transition ease-in-out delay-150 relative w-11/12 px-2 max-w-xs bg-white rounded-lg shadow dark:bg-gray-700">
        <button v-on:click="showModalSucess = false" type="button" class="absolute top-3 right-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ml-auto inline-flex items-center dark:hover:bg-gray-800 dark:hover:text-white" data-modal-toggle="authentication-modal">
          <svg aria-hidden="true" class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg>
          <span class="sr-only">Close modal</span>
        </button>
        <div class="py-6 px-6 flex flex-wrap content-center justify-center">
          <div class="w-16 mb-6">
            <IconSucess/>
          </div>
          <div>
            <h3 class="mb-4 text-xl font-medium text-gray-900 dark:text-white">{{menssageModalSucess}}</h3>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import LoginImage from '../../assets/svg/ImageLogin.vue'
  import IconSucess from '../../assets/svg/IconSucess.vue'
  import ImageLogin from '../../assets/svg/ImageLogin.vue'

  export default {
    name: 'LoginContainer',

    components: {
      LoginImage,
      ImageLogin,
      IconSucess
    },

    computed: {
      ...mapGetters({})
    },

    data() {
      return {
        showModalUpdate: false,
        showModalregisteredSuccessfully: true,
        showModalSignInSuccessfully: false,
        showModalForgetPassword: false,
        showModalSucess: false,
        showLoginMensageAlert: false,
        ShowUpdateMensageAlert: false,
        menssageModalSucess: '',
        UpdateMensageAlert: '',
        email: '',
        password: '',
        UpdateEmail: '',
        UpdatePassword: '',
        UpdateConfirmPassword: '',
        modalUpdateTitle: '',
        modalUpdateButton: '',
        modalUpdatePassword: '',
        modalTypeState: '',
      }
    },

    methods: {

      onClickSignIn: function(){
        const storageEmail = localStorage.email
        const storagePassword = localStorage.password

        if(this.email !== storageEmail || this.password !== storagePassword){
          this.showLoginMensageAlert = true 
          return
        }

        this.showLoginMensageAlert = false 
        this.onShowModalSucess('Login realizado com sucesso!')
      },

      onClickUpdateAccount: function(){
        const storageEmail = localStorage.email

        if(this.modalTypeState === 'recovery' && this.UpdateEmail !== storageEmail){
          this.onShowUpdateMensageAlert('Email não registrado!') 
          return
        }
 
        if(this.UpdateEmail === '' || this.UpdatePassword === '' || this.UpdateConfirmPassword === ''){
          this.onShowUpdateMensageAlert('Preencha todos os campos!') 
          return
        }

        if(this.UpdatePassword !== this.UpdateConfirmPassword){
          this.onShowUpdateMensageAlert('As senhas não correspondem!')
          return
        }

        localStorage.email = this.UpdateEmail
        localStorage.password = this.UpdatePassword

        this.clearUpdate()

        this.showModalUpdate = false 
        this.onShowModalSucess('Cadastro realizado com sucesso!')
      },

      onShowModalUpdate: function(modalType){

        if(modalType === 'signUp'){
          this.modalUpdateTitle = 'Realize seu cadastro'
          this.modalUpdatePassword = 'Senha'
          this.modalUpdateButton = 'Criar conta'
          this.modalTypeState = 'signUp'
        } else if (modalType === 'recovery'){
          this.modalUpdateTitle = 'Atualize sua senha'
          this.modalUpdatePassword = 'Nova senha'
          this.modalUpdateButton = 'Atualizar'
          this.modalTypeState = 'recovery'
        }

        this.showModalUpdate = true
      },

      clearUpdate: function(){
        this.UpdateEmail = ''
        this.UpdatePassword = ''
        this.UpdateConfirmPassword = ''
        this.ShowUpdateMensageAlert = false
        this.showModalSucess = false
      },

      onShowUpdateMensageAlert: function(mensage){
        this.UpdateMensageAlert = mensage
        this.ShowUpdateMensageAlert = true
      },

      onShowModalSucess: function(menssage){
        this.menssageModalSucess = menssage
        this.showModalSucess = true
      },

      onCloseUpdatemodal: function(){
        this.clearUpdate()
        this.showModalUpdate = false
      }
    }
  }
</script>