<template>
  <v-card class="mx-auto" outlined>
    <v-card-title class="primary--text white">
      <v-icon color="primary" class="mx-2"> mdi-email </v-icon>
      <div>
        {{ $t("strings.asset-email-title") }}
      </div>
    </v-card-title>
    <v-card-text>
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-text-field
          v-model="email.to"
          :rules="[rules.required, rules.email]"
          name="emailTo"
          :label="$t('labels.to')"
          :hint="$t('strings.email-to-hint')"
          prepend-icon="mdi-account"
          required
          outlined
          dense
        ></v-text-field>

        <v-text-field
          v-model="email.cc"
          :rules="[rules.email]"
          name="emailCc"
          :label="$t('labels.cc')"
          :hint="$t('strings.email-to-hint')"
          prepend-icon="mdi-account-outline"
          outlined
          dense
        ></v-text-field>

        <v-text-field
          v-model="email.subject"
          :rules="[rules.required, rules.subject]"
          :label="$t('labels.subject')"
          :hint="$t('strings.email-subject-hint')"
          name="emailSubject"
          prepend-icon="mdi-bullseye"
          required
          outlined
          dense
        ></v-text-field>
        <v-textarea
          v-model="email.body"
          name="emailBody"
          :label="$t('labels.text')"
          :hint="$t('strings.email-body-hint')"
          prepend-icon="mdi-text-box-edit"
          outlined
          dense
          hide-details
          :no-resize="true"
        ></v-textarea>
      </v-form>
    </v-card-text>
    <v-card-actions>
      <v-spacer></v-spacer>
      <v-btn
        outlined
        small
        rounded
        color="secondary"
        class="ml-2"
        @click="reset"
      >
        {{ $t("labels.cancel") }}
      </v-btn>
      <v-btn
        :disabled="!valid"
        outlined
        rounded
        small
        color="secondary"
        class="ml-2"
        @click="validate"
      >
        {{ $t("labels.save") }}
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: ["selectedAction"],
  data: function () {
    return {
      email: {
        to: [],
        cc: [],
        subject: "",
        body: "",
      },
      valid: true,
      rules: {
        required: (v) => !!v || this.$t("strings.required-field"),
        email: (v) =>
          /.+@.+\..+/.test(v) || this.$t("strings.valid-email-format"),
        subject: (v) =>
          (!!v && v.length <= 100) || this.$t("strings.required-subject"),
      },
    };
  },
  methods: {
    selectItem(item) {
      this.$emit("select-item", item);
    },
    validate() {
      if (this.$refs.form.validate()) {
        this.setAction();
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    setAction() {
      this.selectItem(this.email);
      this.resetValidation();
      this.reset();
    },
  },
};
</script>
