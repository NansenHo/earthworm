<template>
  <slot></slot>
</template>

<script setup lang="ts">
import {
  injectHttpStatusErrorHandler,
} from "~/api/http";

useHttpStatusError();

function useHttpStatusError() {
  const message = useMessage();
  const router = useRouter();

  injectHttpStatusErrorHandler(async (errMessage, statusCode) => {
    switch (statusCode) {
      case 400:
        message.error(errMessage);
        break;
      case 401:
        message.error(errMessage, {
          duration: 500,
          onLeave() {
            const callback = window.location.pathname;
            router.push(`/auth/login?callback=${callback}`);
          },
        });
        break;
    }
  });
}
</script>

<style scoped></style>
