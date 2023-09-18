<script>
import { ref } from "vue";

export default {
  data() {
    return {
      cep: "",
    };
  },
  methods: {
    cepFetch() {
      const cepPattern = /^\d{5}-\d{3}$/;
      const url = `https://brasilapi.com.br/api/cep/v2/${this.cep}`;
      if (cepPattern.test(this.cep)) {
        try {
          fetch(url)
            .then((response) => response.json())
            .then((data) => {
              if (!data.street) {
                alert("CEP Incorreto. Tente novamente.");
                this.cep = "";
                return;
              }
              const rua = document.querySelector(".rua");
              rua.classList.remove("hide");
              rua.innerText = data.street;

              const bairro = document.querySelector(".bairro");
              bairro.innerText = `Bairro: ${data.neighborhood}`;
              bairro.classList.remove("hide");

              const cidade = document.querySelector(".cidade");
              cidade.innerText = `Cidade: ${data.city}`;
              cidade.classList.remove("hide");

              const estado = document.querySelector(".estado");
              estado.innerText = `Estado: ${data.state}`;
              estado.classList.remove("hide");
              this.cep = "";
            });
        } catch (error) {
          console.log("Erro:" + error);
        }
      } else {
        alert("CEP Inválido. Tente novamente.");
        this.cep = "";
        return;
      }
    },
    formatCep() {
      this.cep = this.cep.replace(/\D/g, "");

      if (this.cep.length >= 5) {
        this.cep = this.cep.substring(0, 5) + "-" + this.cep.substring(5, 8);
      }
    },
  },
};
</script>

<template>
  <main class="container">
    <h1>Buscador de Endereço</h1>
    <div class="inputs">
      <input
        type="text"
        name="cep"
        id="cep"
        v-model="cep"
        maxlength="9"
        @input="formatCep"
        placeholder="Digite um cep"
      />
      <button @click="cepFetch" id="btn" type="submit">Buscar</button>
    </div>

    <div class="texts">
      <p class="rua hide"></p>
      <p class="bairro hide"></p>
      <p class="cidade hide"></p>
      <p class="estado hide"></p>
    </div>
  </main>
</template>

<style>
.container {
  display: flex;
  align-items: center;
  flex-direction: column;
  gap: 1.5rem;
  width: 500px;
  height: 550px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #8136c7;
  border-radius: 5px;
}

.container h1 {
  font-family: Verdana;
  color: white;
  margin-top: 2rem;
  font-size: 30px;
}

.container .inputs {
  padding-bottom: 20px;
  border-bottom: 1px solid #ccc;
}

.container .inputs input[type="text"],
button {
  padding: 10px 20px;
  border-radius: 5px;
  outline: none;
}

.container .inputs input[type="text"] {
  margin-right: 10px;
  border: none;
}

.container .inputs button {
  border: none;
  cursor: pointer;
}

.container .inputs button:hover {
  background-color: #e7e5e5;
}

.container .texts {
  margin-top: 2rem;
}

.container .texts p {
  font-family: sans-serif;
  padding: 1rem;
  margin-bottom: 30px;
  width: 200px;
  text-align: center;
  background-color: #f0f2f5;
  border: 1px solid black;
  border-radius: 5px;
}

.hide {
  display: none;
}
</style>
