<script setup>
import { SHA256 } from 'crypto-js';
import router from "../../router/index.js";

const formData = {
  email: '',
  address: '',
  birth: '',
  lastName: '',
  firstName: '',
  password: '',
  phone: '',
  role:'customer'
};

async function submitForm(event) {
  formData.password = SHA256(formData.password).toString();
  event.preventDefault(); // Empêche le comportement par défaut du formulaire
  // Accédez aux données du formulaire via this.formData
  console.log('Données du formulaire:', formData);

  try {
    const response = await fetch(import.meta.env.VITE_ENDPOINT_URL+"user/register", {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(formData)
    });
    if (response.ok) {
      console.log('Le formulaire a été soumis avec succès !');
      router.push({path:'/login'})
    } else {
      console.log('Une erreur s\'est produite lors de la soumission du formulaire.');
      console.table(response)
    }
  } catch (error) {
    console.error('Une erreur s\'est produite lors de la requête POST :', error);
  }

}
</script>

<template>
  <div class="main-div">
    <img :src="`/img/cesi_eat.png`" />
    <div class="text">
      Veuillez entrer vos informations
    </div>
    <form @submit="submitForm" class="form">
      <div class="form"> Nom</div>
      <input v-model.trim="formData.lastName" type="text" name="Lname" id="Lname" required>
      <div class="form"> Prénom</div>
      <input v-model.trim="formData.firstName" type="text" name="Fname" id="Fname" required>
      <div class="form"> Adresse e-mail</div>
      <input v-model.trim="formData.email" type="email" name="email" id="email" required>
      <div class="form">Mot de passe</div>
      <input v-model.trim="formData.password" type="password" name="pwd" id="pwd" minlength="7" required>
      <div class="form">Adresse</div>
      <textarea v-model.trim="formData.address" name="Address" id="address" required></textarea>
      <div class="form">Numéro de téléphone</div>
      <input v-model.trim="formData.phone" type="phone" name="phone" id="phone" required>
      <div class="form">Date de naissance (facultatif)</div>
      <input v-model="formData.birth" type="date" name="birth" id="birth">

      <input type="submit" value="Valider">
    </form>
    <router-link class="link" to="/logIn">Vous avez déjà un compte ? Cliquez ici pour vous connecter</router-link>
  </div>
</template>

<style scoped>
body {

  background-color: #272625;
  color: #ECB056;
  align-items: center;
  justify-content: center;
  height: 98vh;
  margin: 0;
  font-size: medium;
  font-family: sans-serif
}

.link{
  margin-top: 25px;
    color:#ECB056;
    text-align: center;
}

.main-div {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

input {
  width: 60%;
  padding: 8px;
  margin-bottom: 10px;
}

form{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 75%;
  max-height: 75%;
}

input[type="submit"]{
  background-color: #ECB056;
  border: 2px solid #1A1A1A;
  border-radius: 15px;
  box-sizing: border-box;
  color: #FFFFFF;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  min-height: 25px;
  min-width: 0;
  outline: none;
  padding: 14px 22px;
  text-align: center;
  text-decoration: none;
  transition: all 300ms cubic-bezier(.23, 1, 0.32, 1);
  touch-action: manipulation;
  width: 63%;
  will-change: transform;
  margin-top: 8px;
  align-items: flex-end;
}

button {
  background-color: #ECB056;
  border: 2px solid #1A1A1A;
  border-radius: 15px;
  box-sizing: border-box;
  color: #FFFFFF;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  min-height: 25px;
  min-width: 0;
  outline: none;
  padding: 14px 22px;
  text-align: center;
  text-decoration: none;
  transition: all 300ms cubic-bezier(.23, 1, 0.32, 1);
  touch-action: manipulation;
  width: 35%;
  will-change: transform;
  margin-top: 6px;
  align-items: flex-end;
}

button:disabled {
  pointer-events: none;
}

button:hover {
  box-shadow: rgba(0, 0, 0, 0.25) 0 8px 15px;
  transform: translateY(-2px);
}

button:active {
  box-shadow: none;
  transform: translateY(0);
}

.text {
  margin-bottom: 15px;
  align-self: center;
  color: #ECB056;
  font-size: large;
}

img {
  width: 50%;
  height: auto;
}

.form {
  text-align: center;
  color: #ECB056;
  font-family: sans-serif;
  font-size: large;
}

textarea{
  min-width: 63%;
  max-width: 63%;
}
</style>
