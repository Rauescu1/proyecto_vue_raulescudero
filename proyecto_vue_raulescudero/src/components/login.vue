<script setup>
import { ref } from "vue";
import { auth } from "../firebase.js"
import { signInWithEmailAndPassword, GoogleAuthProvider, signInWithPopup } from "firebase/auth";
let usuario = ref(""), password = ref("");

function loginUsuario() {
    signInWithEmailAndPassword(auth, usuario.value, password.value)
        .then((userCredential) => {
            // Signed in 
            const user = userCredential.user;
            // ...
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
        });
}
function loginGoogle() {
    signInWithPopup(auth, new GoogleAuthProvider())
        .then((result) => {
            // This gives you a Google Access Token. You can use it to access the Google API.
            const credential = GoogleAuthProvider.credentialFromResult(result);
            const token = credential.accessToken;
            // The signed-in user info.
            const user = result.user;
            // IdP data available using getAdditionalUserInfo(result)
            // ...
        }).catch((error) => {
            // Handle Errors here.
            const errorCode = error.code;
            const errorMessage = error.message;
            // The email of the user's account used.
            const email = error.customData.email;
            // The AuthCredential type that was used.
            const credential = GoogleAuthProvider.credentialFromError(error);
            // ...
        });

}
</script>
<template>
    Usuario: <input type="text" v-model="usuario">
    Password: <input type="password" v-model="password">
    <p><button @click="loginUsuario">LOGIN :)</button></p>
    <button @click="loginGoogle">GOOGLE</button>
</template>
