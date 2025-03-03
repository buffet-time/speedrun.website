<template>
  <Card id="signUpCard" data-testid="sign-up-card" class="signup-card">
    <CardHeader class="signup-card__header">
      <BaseButton
        id="login-button"
        class="signup-card__login-button"
        @click="$emit('logInClick')"
      >
        Log In
      </BaseButton>

      <div class="signup-card__title">Sign Up</div>

      <CloseButton v-show="modal" @click.prevent="$emit('close')" />
    </CardHeader>
    <div class="signup-card__content">
      <div class="signup-card__content-wrapper">
        <BaseInput
          v-model="email"
          name="email"
          type="text"
          placeholder="Email"
          autocomplete="email"
        />

        <div class="signup-card__input-wrapper">
          <BaseInput
            v-model="username"
            name="username"
            type="text"
            placeholder="Username"
            autocomplete="nickname"
          />
          <p>This can be changed later</p>
        </div>

        <div class="signup-card__input-wrapper">
          <div class="signup-card__password-wrapper">
            <BaseInput
              v-model="password"
              name="password"
              class="signup-card__password-field"
              :type="showPassword ? 'text' : 'password'"
              placeholder="Password"
              autocomplete="password"
            />

            <BaseInput
              v-model="passwordConfirm"
              name="passwordConfirm"
              class="signup-card__password-field"
              :type="showPassword ? 'text' : 'password'"
              placeholder="Confirm"
              autocomplete="password"
            />

            <HideShowPassword
              id="hide-show-password"
              type="button"
              @click="showPassword = !showPassword"
              @keydown.enter.prevent
              @keyup.enter="showPassword = !showPassword"
            />
          </div>

          <p>* Must be 8 characters with a mix of letters and numbers</p>
        </div>

        <BaseButton class="signup-button" @click="signup"> Sign Up </BaseButton>
      </div>

      <div class="signup-card__auth-buttons">
        <BaseButton class="signup-button">
          <svg-icon class="mr-2 w-5 h-5" name="github" />

          <p>Sign Up with Github</p>
        </BaseButton>

        <BaseButton class="signup-button">
          <svg-icon class="mr-2 w-5 h-5" name="google" />

          <p>Sign Up with Google</p>
        </BaseButton>
      </div>
    </div>
  </Card>
</template>

<script lang="ts">
import {
  defineComponent,
  reactive,
  toRefs,
  useContext,
} from '@nuxtjs/composition-api';
import HideShowPassword from '@/components/elements/buttons/HideShowPassword/HideShowPassword.vue';
import BaseButton from '@/components/elements/buttons/BaseButton.vue';
import Card from '@/components/elements/cards/Card.vue';
import CardHeader from '@/components/elements/cards/CardHeader.vue';
import CloseButton from '@/components/elements/buttons/CloseButton.vue';
import BaseInput from '@/components/elements/BaseInput.vue';
import { UserRegister } from '@/types';

export default defineComponent({
  components: {
    BaseButton,
    BaseInput,
    Card,
    CardHeader,
    CloseButton,
    HideShowPassword,
  },
  props: {
    modal: {
      type: Boolean,
      required: false,
      default: false,
    },
  },
  setup(_, { emit }) {
    const { $api } = useContext();

    const register = reactive<UserRegister>({
      email: '',
      password: '',
      passwordConfirm: '',
      username: '',
    });

    const state = reactive({
      showPassword: false,
    });

    async function signup() {
      await $api.register<void>(register);

      register.email = '';
      register.password = '';
      register.passwordConfirm = '';
      register.username = '';
      state.showPassword = false;

      emit('logInClick');
    }

    return {
      ...toRefs(state),
      ...toRefs(register),
      signup,
    };
  },
});
</script>

<style lang="postcss" scoped>
.signup-card {
  @apply bg-white w-full max-w-xl;
  & .signup-card__header {
    @apply flex flex-row space-x-3;
  }
  & .signup-card__title {
    @apply flex flex-1 justify-center px-3 py-2 rounded bg-gray-100 text-gray-900;
  }
  & .signup-card__login-button {
    @apply flex flex-1 justify-center bg-white text-gray-900 border border-gray-300;
  }
  & .signup-card__content {
    @apply p-3;
  }
  & .signup-card__content-wrapper {
    @apply flex flex-col space-y-3 pb-3 mb-3 border-b border-gray-300;
  }
  & .signup-card__input-wrapper {
    @apply flex flex-1 flex-col space-y-1;
  }
  & .signup-card__password-wrapper {
    @apply flex flex-1 flex-col sm:flex-row space-y-3 sm:space-y-0 sm:space-x-2;
  }
  & .signup-card__password-field {
    @apply grow;
  }
  & .signup-card__auth-buttons {
    @apply flex flex-col w-full space-y-2;
  }
  & p {
    @apply text-sm;
  }
}
#hide-show-password {
  @apply text-gray-700 hover:bg-gray-100;
}

#login-button {
  @apply hover:bg-gray-100;
}

.signup-button {
  @apply flex flex-1 items-center justify-center fill-current bg-gray-100 text-gray-900 hover:bg-gray-200;
}
</style>
