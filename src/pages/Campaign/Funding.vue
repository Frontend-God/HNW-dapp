<template>
  <div>
    <campaign-breadcrumb :home="home" :title="title" @saveCampaign="save" />
    <div class="card bg-dark text-white p-4 mt-4">
      <div class="row">
        <div class="col-12 col-lg-10">
          <h2>Funding Type</h2>
          <div class="btn-group my-4">
            <button
              class="btn"
              :class="[
                type === 'softCap'
                  ? 'active btn-primary'
                  : 'btn-info text-dark',
              ]"
              @click="setFundingInfo({ type: 'softCap' })"
            >
              Soft Cap
            </button>
            <button
              class="btn"
              :class="[
                type === 'flexNoMinCap'
                  ? 'active btn-primary'
                  : 'btn-info text-dark',
              ]"
              @click="setFundingInfo({ type: 'flexNoMinCap' })"
            >
              Flex no min cap
            </button>
          </div>
          <div class="mt-4">
            <h2>
              Campaign Goal Amount <span class="entreField-required">*</span>
            </h2>
            <p class="fs-5">
              How much money would you like to raise for this campaign?
            </p>
            <div class="entreField">
              <input
                class="
                  form-control form-control-lg
                  bg-transparent
                  border border-primary
                "
                type="number"
                min="0"
                v-model="amount"
              />
              <span class="error-message" v-if="errors.amount">{{
                errors.amount
              }}</span>
            </div>
          </div>
          <div class="mt-4 pt-4 border-top border-primary">
            <h2>
              Token to raise in <span class="entreField-required">*</span>
            </h2>
            <p class="fs-5">Token to raise in</p>
            <div class="entreField">
              <input
                class="
                  form-control form-control-lg
                  bg-transparent
                  border border-primary
                "
                type="text"
                disabled
              />
            </div>
          </div>
          <div class="mt-4 pt-4 border-top border-primary">
            <h2>Wallet Address <span class="entreField-required">*</span></h2>
            <p class="fs-5">Fill out your address to send funds to.</p>
            <div class="entreField">
              <input
                class="
                  form-control form-control-lg
                  bg-transparent
                  border border-primary
                "
                type="text"
                v-model="address"
              />
              <span class="error-message" v-if="errors.address">{{
                errors.address
              }}</span>
            </div>
          </div>
          <div
            class="
              d-flex
              justify-content-end
              save-content
              border-top border-primary
              w-100
            "
          >
            <button class="btn btn-primary text-white fs-6" @click="save">
              Save & Continue
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
import CampaignBreadcrumb from "../../components/CampaignBreadcrumb.vue";
export default {
  components: { CampaignBreadcrumb },
  data() {
    return {
      home: "Campaign",
      title: "Funding",
      errors: {
        amount: "",
        token: "",
        address: "",
      },
    };
  },
  computed: {
    ...mapState(["campaigns"]),
    type: {
      set(type) {
        this.setFundingInfo({ type });
      },
      get() {
        return this.campaigns.funding.type;
      },
    },
    amount: {
      set(amount) {
        this.setFundingInfo({ amount });
      },
      get() {
        return this.campaigns.funding.amount;
      },
    },
    token: {
      set(token) {
        this.setFundingInfo({ token });
      },
      get() {
        return this.campaigns.funding.token;
      },
    },
    address: {
      set(address) {
        this.setFundingInfo({ address });
      },
      get() {
        return this.campaigns.funding.address;
      },
    },
  },
  methods: {
    ...mapActions({
      setFundingInfo: "campaigns/setFundingInfo",
    }),
    save: function () {
      if (!this.validate()) {
        this.showAlert({
          type: "error",
          title: "Error",
          html: "Please fix the issues!",
        });
      } else {
        this.$router.push("/campaign_edit/settings");
      }
    },
    validate: function () {
      let isValid = true;

      if (!this.amount || this.amount <= 0) {
        isValid = false;
        if (this.amount <= 0)
          this.errors.amount = "Amount should be higher than 0";
        if (!this.amount) this.errors.amount = "Amount is required";
      } else this.errors.amount = "";

      if (!this.address) {
        isValid = false;
        this.errors.address = "Address is required";
      } else this.errors.address = "";

      return isValid;
    },
    showAlert: function ({ title = "", type = "", html = "" }) {
      this.$swal.fire({
        icon: type,
        title,
        html,
        showConfirmButton: true,
        timer: 3000,
      });
    },
  },
};
</script>

<style>
.container {
  width: 100%;
}
</style>
