<template>
  <div id="userLoginView">
    <h2>Welcome to login!</h2>
    <a-form :model="form" :style="{}" @submit="handleSubmit">
      <a-form-item field="userAccount" label="UserAccount">
        <a-input
          v-model="form.userAccount"
          placeholder="please enter your userAccount"
        />
      </a-form-item>
      <a-form-item
        field="userPassword"
        tooltip="The password is not less than 8 digits."
        label="UserPassword"
      >
        <a-input-password
          v-model="form.userPassword"
          placeholder="please enter your userPassword"
        />
      </a-form-item>
      <a-form-item>
        <a-button html-type="submit">Submit</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserLoginRequest } from "../../../generated";
import message from "@arco-design/web-vue/es/message";
import { useRouter } from "vue-router";
import { useStore } from "vuex";

/**
 * 表单信息
 */
const form = reactive({
  userAccount: "",
  userPassword: "",
} as UserLoginRequest);

const router = useRouter();
const store = useStore();

/**
 * 提交表单
 */
const handleSubmit = async () => {
  const res = await UserControllerService.userLogoutUsingPost(form);
  if (res.code === 0) {
    //登陆成功跳转到主页
    await store.dispatch("user/getLoginUser");
    await router.push({
      path: "/",
      replace: true,
    });
  } else {
    message.error("登录失败" + res.message);
  }
};
</script>
