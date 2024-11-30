<template>
  <!-- Conteneur principal centré -->
  <div class="flex items-center justify-center min-h-screen bg-gray-100">
    <!-- Boîte contenant le formulaire -->
    <div class="w-full max-w-lg p-10 bg-aliceblue border border-gray-300 rounded-lg shadow-lg">
      <h2 class="text-3xl font-bold text-center mb-8 text-gray-800">Formulaire de paiement</h2>

      <form @submit.prevent="handleSubmit">
        <!-- Champ Montant -->
        <div class="mb-6">
          <label for="amount" class="block text-lg font-medium text-gray-700">Montant</label>
          <input
            type="number"
            id="amount"
            v-model="transaction.amount"
            required
            class="w-full p-4 border border-gray-300 rounded-lg text-lg focus:ring-blue-500 focus:border-blue-500"
            :class="{ 'border-red-500': errors.amount }"
          />
          <p v-if="errors.amount" class="text-red-500 text-sm mt-2">Le montant doit être valide.</p>
        </div>

        <!-- Champ Méthode de paiement -->
        <div class="mb-6">
          <label class="block text-lg font-medium text-gray-700">Méthode de paiement</label>
          <div class="flex gap-6 mt-3">
            <label class="flex items-center cursor-pointer">
              <input
                type="radio"
                v-model="transaction.paymentMethod"
                value="mobile_money"
                required
                class="form-radio text-blue-500"
              />
              <span class="ml-3 text-lg text-gray-700">Mobile Money</span>
            </label>
            <label class="flex items-center cursor-pointer">
              <input
                type="radio"
                v-model="transaction.paymentMethod"
                value="card"
                required
                class="form-radio text-blue-500"
              />
              <span class="ml-3 text-lg text-gray-700">Carte bancaire</span>
            </label>
          </div>
          <p v-if="errors.paymentMethod" class="text-red-500 text-sm mt-2">
            Veuillez choisir une méthode de paiement.
          </p>
        </div>

        <!-- Champs Informations client -->
        <div class="mb-6">
          <label for="name" class="block text-lg font-medium text-gray-700">Nom</label>
          <input
            type="text"
            id="name"
            v-model="transaction.customer.name"
            required
            class="w-full p-4 border border-gray-300 rounded-lg text-lg focus:ring-blue-500 focus:border-blue-500"
          />
        </div>

        <div class="mb-6">
          <label for="email" class="block text-lg font-medium text-gray-700">Email</label>
          <input
            type="email"
            id="email"
            v-model="transaction.customer.email"
            required
            class="w-full p-4 border border-gray-300 rounded-lg text-lg focus:ring-blue-500 focus:border-blue-500"
          />
        </div>

        <div class="mb-6">
          <label for="phone" class="block text-lg font-medium text-gray-700">Téléphone</label>
          <input
            type="tel"
            id="phone"
            v-model="transaction.customer.phone"
            required
            class="w-full p-4 border border-gray-300 rounded-lg text-lg focus:ring-blue-500 focus:border-blue-500"
          />
        </div>

        <!-- Bouton de soumission -->
        <button
          type="submit"
          :disabled="isFormInvalid"
          class="w-full p-4 bg-blue-500 text-white text-lg font-semibold rounded-lg hover:bg-blue-600 transition disabled:bg-gray-300"
        >
          Effectuer le paiement
        </button>
      </form>

      <div v-if="loading" class="mt-6 text-center text-lg">Chargement...</div>
      <div v-if="status" class="mt-6 text-center">
        <p :class="{ 'text-green-500': status === 'success', 'text-red-500': status === 'failed' }" class="text-lg">
          {{ status === 'success' ? 'Paiement réussi!' : 'Échec du paiement.' }}
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      transaction: {
        amount: 0,
        paymentMethod: '',
        customer: {
          name: '',
          email: '',
          phone: '',
        },
        status: 'pending',
      },
      errors: {},
      loading: false,
      status: null,
    };
  },
  computed: {
    isFormInvalid() {
      return (
        !this.transaction.amount ||
        !this.transaction.paymentMethod ||
        !this.transaction.customer.name ||
        !this.transaction.customer.email ||
        !this.transaction.customer.phone
      );
    },
  },
  methods: {
    handleSubmit() {
      this.loading = true;
      this.errors = {};

      if (this.validateForm()) {
        setTimeout(() => {
          this.loading = false;
          this.status = 'success'; // Simuler un paiement réussi
        }, 2000);
      } else {
        this.loading = false;
      }
    },

    validateForm() {
      let valid = true;
      if (this.transaction.amount <= 0) {
        this.errors.amount = true;
        valid = false;
      }
      if (!this.transaction.paymentMethod) {
        this.errors.paymentMethod = true;
        valid = false;
      }
      return valid;
    },
  },
};
</script>

<style scoped>
.bg-aliceblue {
  background-color: aliceblue;
}
</style>
