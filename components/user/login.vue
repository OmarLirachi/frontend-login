<template>
    <v-card class="cardLogin">
        <v-card-title class="title">Wuelcon to el barsa</v-card-title>
        <v-card-text>
            <v-row justify="center" align="center">
                <v-col cols="4" justify="center" align-self="center">
                    <img src="../../assets/images/goku.png" class="imgLogin">
                </v-col>
                <v-col cols="8" justify="center" align-self="center">
                    <v-form ref="formLogin">
                <v-text-field label="Ingresa tu correo electronico" 
                              placeholder="ejemplo: hola@outlook.com"
                              v-model="correoElectronico"
                              :rules="validarCorreo"
                />
                <v-text-field label="Password" 
                              placeholder="Password"
                              v-model="password"
                              :rules="validarPassword"
                />
            </v-form>
                </v-col>
            </v-row>
            
        </v-card-text>
        <v-card-actions>
            <v-btn
            class="btnLogin"
            rounded
            block
            @click="loginBackend"
            >
                Iniciar
            </v-btn>
        </v-card-actions>
    </v-card>
</template>

<script>
export default {
    data() {
        return {
            correoElectronico: '',
            validarCorreo: [
            v => !v || /^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
            ],
            password:'',
            validarPassword: [
                value => value.length >= 6 || 'Min 6 characters'
            ]
        }
    },
    methods: {
        async loginBackend () {
            const valid = this.$refs.formLogin.validate()
            if (valid) {
                const sendData = {
                    email: this.correoElectronico,
                    password: this.password
                }
                await this.$auth.loginWith('local', {
                    data: sendData
                }).then(async (res)=> {
                    console.log('respuesta del back', res)
                    if (res.data.error == null) {
                        this.$router.push('/dashboard')
                    }
                }).catch((error)=> {
                    console.log('error: ', error)
                })
            }else {
                alert('no cumpliste las reglas')
            }
        }
    }
}
</script>

<style scoped>
    .cardLogin{
        background-color: #4894d3;
        border-radius: 10px;
        width: 500px;
        height: 300px;
    }

    .imgLogin {
        width: 100%;
        height: 100%;
    }

    .btnLogin{
        background-color: #cc3737!important;
        color: rgb(255, 255, 255);
    }

    .title {
        font-size: 30px;
        justify-content: center;
        color: #cc3737;
        font-weight: 700;
    }
</style>