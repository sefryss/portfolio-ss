<script setup>
import SectionHeading from "@/components/SectionHeading.vue";
import NButton from "@/components/NButton.vue";
import InputField from "@/components/InputField.vue";
import { reactive, ref } from "vue";
import InputLabel from "@/components/InputLabel.vue";
import TextArea from "@/components/TextArea.vue";
import Toast from "@/components/Toast.vue";

const api = "https://fahim.hasteit.com/api/v1/messages";

let contactForm = reactive({
  name: "",
  email: "",
  subject: "",
  message: "",
});

let contactFormErrors = reactive({
  name: "",
  email: "",
  subject: "",
  message: "",
});

let loading = ref(false);

let showToast = ref(false);
let handleClose = () => {
  showToast.value = false;
};

let submitContactForm = () => {
  // Basic front-end validation
  if (
    !contactForm.name ||
    !contactForm.email ||
    !contactForm.subject ||
    !contactForm.message
  ) {
    alert("Please fill in all fields.");
    return;
  }

  loading.value = true;

  fetch(api, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Accept: "application/json",
    },
    body: JSON.stringify(contactForm),
  })
    .then((response) => {
      if (response.status === 201) {
        // Reset form fields
        Object.keys(contactForm).forEach((key) => (contactForm[key] = ""));
        return response.json();
      } else if (response.status === 422) {
        return response.json();
      } else {
        throw new Error("Something went wrong");
      }
    })
    .then((data) => {
      if (data.errors) {
        contactFormErrors.name = data.errors.name ? data.errors.name[0] : "";
        contactFormErrors.email = data.errors.email ? data.errors.email[0] : "";
        contactFormErrors.subject = data.errors.subject
          ? data.errors.subject[0]
          : "";
        contactFormErrors.message = data.errors.message
          ? data.errors.message[0]
          : "";
      } else {
        // Clear errors and show success toast
        Object.keys(contactFormErrors).forEach(
          (key) => (contactFormErrors[key] = "")
        );
        showToast.value = true;
        setTimeout(() => {
          showToast.value = false;
        }, 10000);
      }
    })
    .catch((error) => {
      console.error(error);
    })
    .finally(() => {
      loading.value = false;
    });
};
</script>

<template>
  <transition name="fade" mode="in-out">
    <Toast
      v-if="showToast"
      :message="'Thank You, for your message! Will get back to you shortly.'"
      @close="handleClose"
    />
  </transition>

  <!-- Contact -->
  <section id="contact" class="relative py-28 lg:py-36 overflow-hidden">
    <div
      class="absolute top-44 -left-64 h-[250px] w-[900px] -rotate-25 rounded-3xl bg-gradient-to-r from-cyan-600 to-indigo-800 opacity-20 dark:opacity-20 blur-3xl filter block"
    ></div>
    <div class="container px-3 mx-auto max-w-6xl 2xl:max-w-7xl">
      <SectionHeading>Get In Touch</SectionHeading>

      <div
        class="grid grid-cols-1 md:grid-cols-12 gap-10 dark:text-slate-300 tracking-wide"
      >
        <div class="md:col-span-5">
          <h3 class="font-medium text-3xl mb-2 dark:text-white">
            Quick Contact
          </h3>
          <p class="text-lg text-slate-600 dark:text-slate-300">
            Don't like forms? Then whats app me!
            <a
              href="https://wa.me/+8801727659333"
              target="_blank"
              class="font-medium text-secondary-500 hover:text-secondary-600 dark:text-secondary-300 dark:hover:text-secondary-400 transition-all duration-300"
              >click here</a
            >
          </p>
          <ul class="mt-12 space-y-8">
            <li class="flex items-center space-x-4">
              <div>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-7 h-7 dark:text-primary-300 text-primary-500"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M21.75 9v.906a2.25 2.25 0 01-1.183 1.981l-6.478 3.488M2.25 9v.906a2.25 2.25 0 001.183 1.981l6.478 3.488m8.839 2.51l-4.66-2.51m0 0l-1.023-.55a2.25 2.25 0 00-2.134 0l-1.022.55m0 0l-4.661 2.51m16.5 1.615a2.25 2.25 0 01-2.25 2.25h-15a2.25 2.25 0 01-2.25-2.25V8.844a2.25 2.25 0 011.183-1.98l7.5-4.04a2.25 2.25 0 012.134 0l7.5 4.04a2.25 2.25 0 011.183 1.98V19.5z"
                  />
                </svg>
              </div>
              <div>
                <h4 class="text-xl dark:text-white mb-1">Email</h4>
                <p>
                  <a
                    href="mailto:sefrysyahrudins@gmail.com"
                    class="transition-all duration-300 hover:text-secondary-500 dark:hover:text-secondary-300"
                    >sefrysyahrudins@gmail.com</a
                  >
                </p>
              </div>
            </li>
            <li class="flex items-center space-x-4">
              <div>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-7 h-7 dark:text-primary-300 text-primary-500"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 01.865-.501 48.172 48.172 0 003.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0012 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018z"
                  />
                </svg>
              </div>
              <div>
                <h4 class="text-xl dark:text-white mb-1">Social Media</h4>
                <ul class="flex items-center space-x-2">
                  <li>
                    <a href="https://github.com/sefryss" target="_blank">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="26"
                        height="26"
                        viewBox="0 0 24 24"
                        class="fill-current transition-all duration-300 hover:fill-secondary-500 dark:hover:fill-secondary-400"
                      >
                        <path
                          d="M12 0C5.373 0 0 5.373 0 12c0 5.302 3.438 9.8 8.205 11.385.599.111.82-.26.82-.577v-2.152c-3.338.726-4.042-1.416-4.042-1.416-.546-1.387-1.334-1.757-1.334-1.757-1.091-.745.083-.729.083-.729 1.205.085 1.84 1.237 1.84 1.237 1.07 1.833 2.809 1.304 3.495.997.108-.775.418-1.304.76-1.605-2.665-.305-5.467-1.333-5.467-5.93 0-1.31.469-2.381 1.236-3.221-.124-.305-.535-1.533.118-3.194 0 0 1.008-.323 3.301 1.23a11.52 11.52 0 0 1 3.003-.404c1.02.005 2.045.137 3.003.404 2.29-1.553 3.297-1.23 3.297-1.23.654 1.661.243 2.889.119 3.194.77.84 1.235 1.911 1.235 3.221 0 4.61-2.806 5.624-5.478 5.921.43.372.813 1.103.813 2.222v3.293c0 .32.217.694.825.575C20.565 21.795 24 17.298 24 12 24 5.373 18.627 0 12 0z"
                        ></path>
                      </svg>
                    </a>
                  </li>
                  <li>
                    <a
                      href="https://www.linkedin.com/in/sefrysyahrudin/"
                      target="_blank"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="30"
                        height="30"
                        viewBox="0 0 24 24"
                        class="fill-current transition-all duration-300 hover:fill-secondary-500 dark:hover:fill-secondary-400"
                      >
                        <path
                          d="M20 3H4a1 1 0 0 0-1 1v16a1 1 0 0 0 1 1h16a1 1 0 0 0 1-1V4a1 1 0 0 0-1-1zM8.339 18.337H5.667v-8.59h2.672v8.59zM7.003 8.574a1.548 1.548 0 1 1 0-3.096 1.548 1.548 0 0 1 0 3.096zm11.335 9.763h-2.669V14.16c0-.996-.018-2.277-1.388-2.277-1.39 0-1.601 1.086-1.601 2.207v4.248h-2.667v-8.59h2.56v1.174h.037c.355-.675 1.227-1.387 2.524-1.387 2.704 0 3.203 1.778 3.203 4.092v4.71z"
                        ></path>
                      </svg>
                    </a>
                  </li>
                  <li>
                    <a
                      href="https://www.instagram.com/sefryyy_/"
                      target="_blank"
                    >
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        width="26"
                        height="26"
                        viewBox="0 0 24 24"
                        class="fill-current transition-all duration-300 hover:fill-secondary-500 dark:hover:fill-secondary-400"
                      >
                        <path
                          d="M12 2.163c3.204 0 3.584.012 4.849.07 1.17.054 1.97.24 2.427.402a4.92 4.92 0 0 1 1.787 1.042 4.92 4.92 0 0 1 1.042 1.787c.162.457.348 1.256.402 2.427.058 1.265.07 1.645.07 4.849 0 3.204-.012 3.584-.07 4.849-.054 1.17-.24 1.97-.402 2.427a4.92 4.92 0 0 1-1.042 1.787 4.92 4.92 0 0 1-1.787 1.042c-.457.162-1.256.348-2.427.402-1.265.058-1.645.07-4.849.07-3.204 0-3.584-.012-4.849-.07-1.17-.054-1.97-.24-2.427-.402a4.92 4.92 0 0 1-1.787-1.042 4.92 4.92 0 0 1-1.042-1.787c-.162-.457-.348-1.256-.402-2.427C2.175 15.584 2.163 15.204 2.163 12c0-3.204.012-3.584.07-4.849.054-1.17.24-1.97.402-2.427a4.92 4.92 0 0 1 1.042-1.787A4.92 4.92 0 0 1 5.464 2.635c.457-.162 1.256-.348 2.427-.402 1.265-.058 1.645-.07 4.849-.07zm0-2.163C8.755 0 8.35.014 7.083.072 5.813.13 4.773.322 3.966.59a6.924 6.924 0 0 0-2.507 1.643A6.924 6.924 0 0 0 .59 4.746C.322 5.553.13 6.593.072 7.863.014 9.13 0 9.535 0 12s.014 2.87.072 4.137c.058 1.27.25 2.31.518 3.117a6.924 6.924 0 0 0 1.643 2.507 6.924 6.924 0 0 0 2.507 1.643c.807.268 1.847.46 3.117.518 1.267.058 1.672.072 4.137.072s2.87-.014 4.137-.072c1.27-.058 2.31-.25 3.117-.518a6.924 6.924 0 0 0 2.507-1.643 6.924 6.924 0 0 0 1.643-2.507c.268-.807.46-1.847.518-3.117.058-1.267.072-1.672.072-4.137s-.014-2.87-.072-4.137c-.058-1.27-.25-2.31-.518-3.117a6.924 6.924 0 0 0-1.643-2.507 6.924 6.924 0 0 0-2.507-1.643C19.227.322 18.187.13 16.917.072 15.65.014 15.245 0 12 0z"
                        ></path>
                        <path
                          d="M12 5.838A6.162 6.162 0 0 0 5.838 12 6.162 6.162 0 0 0 12 18.162 6.162 6.162 0 0 0 18.162 12 6.162 6.162 0 0 0 12 5.838zm0 10.162A4.005 4.005 0 0 1 8 12a4.005 4.005 0 0 1 4-4 4.005 4.005 0 0 1 4 4 4.005 4.005 0 0 1-4 4zm6.406-10.845a1.44 1.44 0 1 0 0-2.881 1.44 1.44 0 0 0 0 2.881z"
                        ></path>
                      </svg>
                    </a>
                  </li>
                </ul>
              </div>
            </li>
            <li class="flex items-center space-x-4">
              <div>
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke-width="1.5"
                  stroke="currentColor"
                  class="w-7 h-7 text-primary-500 dark:text-primary-300"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    d="M9 6.75V15m6-6v8.25m.503 3.498l4.875-2.437c.381-.19.622-.58.622-1.006V4.82c0-.836-.88-1.38-1.628-1.006l-3.869 1.934c-.317.159-.69.159-1.006 0L9.503 3.252a1.125 1.125 0 00-1.006 0L3.622 5.689C3.24 5.88 3 6.27 3 6.695V19.18c0 .836.88 1.38 1.628 1.006l3.869-1.934c.317-.159.69-.159 1.006 0l4.994 2.497c.317.158.69.158 1.006 0z"
                  />
                </svg>
              </div>
              <div>
                <h4 class="text-xl dark:text-white mb-1">Location</h4>
                <p>
                  <a
                    href="https://www.google.com/maps/place/Semarang/"
                    target="_blank"
                    class="transition-all duration-300 hover:text-secondary-500 dark:hover:text-secondary-300"
                    >Semarang, Indonesia</a
                  >
                </p>
              </div>
            </li>
          </ul>
        </div>

        <div class="md:col-span-7">
          <form
            @submit.prevent="submitContactForm"
            class="grid grid-cols-2 gap-x-6 gap-y-8"
          >
            <div class="col-span-2 md:col-span-1 flex flex-col space-y-3">
              <InputLabel for="name" title="Name" :required="true" />
              <InputField
                v-model="contactForm.name"
                type="text"
                id="name"
                name="name"
                placeholder="Enter your name"
                :error="contactFormErrors.name"
              />
              <span
                class="text-red-600 text-sm font-medium"
                v-if="contactFormErrors.name"
                >{{ contactFormErrors.name }}</span
              >
            </div>
            <div class="col-span-2 md:col-span-1 flex flex-col space-y-3">
              <InputLabel for="email" title="Email" :required="true" />
              <InputField
                v-model="contactForm.email"
                type="text"
                id="email"
                name="email"
                placeholder="Enter your email"
                :error="contactFormErrors.email"
              />
              <span
                class="text-red-600 text-sm font-medium"
                v-if="contactFormErrors.email"
                >{{ contactFormErrors.email }}</span
              >
            </div>
            <div class="col-span-2 flex flex-col space-y-3">
              <InputLabel for="subject" title="Subject" />
              <InputField
                v-model="contactForm.subject"
                type="text"
                id="subject"
                name="subject"
                placeholder="Enter message subject (optional)"
                :error="contactFormErrors.subject"
              />
              <span
                class="text-red-600 text-sm font-medium"
                v-if="contactFormErrors.subject"
                >{{ contactFormErrors.subject }}</span
              >
            </div>
            <div class="col-span-2 flex flex-col space-y-3">
              <InputLabel for="message" title="Message" :required="true" />
              <TextArea
                v-model="contactForm.message"
                id="message"
                class="h-40"
                name="name"
                placeholder="Enter your message"
                :error="contactFormErrors.message"
              />
              <span
                class="text-red-600 text-sm font-medium"
                v-if="contactFormErrors.message"
                >{{ contactFormErrors.message }}</span
              >
            </div>
            <div class="col-span-2 flex">
              <NButton
                :loading="loading"
                :disabled="loading"
                type="submit"
                btn-type="filled"
                class="px-5 py-3 text-md"
              >
                <span class="mr-1">Send Message</span>
                <template #icon>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="fill-current w-5 h-5"
                    viewBox="0 0 16 16"
                  >
                    <path
                      d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855H.766l-.452.18a.5.5 0 0 0-.082.887l.41.26.001.002 4.995 3.178 3.178 4.995.002.002.26.41a.5.5 0 0 0 .886-.083l6-15Zm-1.833 1.89L6.637 10.07l-.215-.338a.5.5 0 0 0-.154-.154l-.338-.215 7.494-7.494 1.178-.471-.47 1.178Z"
                    />
                  </svg>
                </template>
              </NButton>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
  <!-- Contact -->
</template>

<style scoped>
.fade-enter-from,
.fade-leave-to {
  transform: translateY(-120px);
  opacity: 0;
}

.fade-enter-active,
.fade-leave-active {
  transition-duration: 300ms;
  transition-property: transform, opacity;
  transition-timing-function: cubic-bezier(0.6, 0.15, 0.35, 0.8);
}
</style>
