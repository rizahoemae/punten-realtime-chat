<script setup>
import { reactive } from "vue";

const conversations = ref([
  {
    id: 1,
    sender: "Person 1",
    image: "https://flowbite.com/docs/images/people/profile-picture-5.jpg",
    messages: [
      {
        sender: "Person 1",
        message: "Punten",
      },
      {
        sender: "Person 1",
        message: "Apakabs?",
      },
      {
        sender: "You",
        message: "Mangga!",
      },
      {
        sender: "You",
        message: "Alhamdulillah baik :)",
      },
    ],
  },
  {
    id: 2,
    sender: "Cat 1",
    image:
      "https://i.pinimg.com/564x/65/4e/5d/654e5dfd5a65a81dea2ec1e80a18a03e.jpg",
    messages: [
      {
        sender: "Cat 1",
        message: "skajskajskjs1oio2iisoaisoaisosais",
      },
      {
        sender: "You",
        message: "????",
      },
      {
        sender: "Cat 1",
        message: "!saskjasoqijjfds982dsiusu210902139023",
      },
      {
        sender: "You",
        message: "Hmmm?",
      },
    ],
  },
  {
    id: 3,
    sender: "Join if you're a cat",
    image:
      "https://i.pinimg.com/564x/ad/67/04/ad67042e13ecb331cede6bc41d6e3008.jpg",
    type: "group",
    messages: [
      {
        sender: "Cat 1",
        message: "Hello!",
        isFriend: true,
        image:
          "https://i.pinimg.com/564x/65/4e/5d/654e5dfd5a65a81dea2ec1e80a18a03e.jpg",
      },
      {
        sender: "+62812345678",
        message: "Rawr",
        isFriend: false,
        image:
          "https://i.pinimg.com/564x/5c/b9/78/5cb978c7b18ae304a0c5f8532a76bb0a.jpg",
      },
    ],
  },
]);

const addFriend = () => {
  conversations.value[2].messages[1].isFriend = true;
  modal.addFriend = false;
};
const selected = ref();
const message = ref("");
const isReceiving = ref(false);
const modal = reactive({
  addFriend: false,
});

let intervalId;

const sendMessage = () => {
  let obj = {
    sender: "You",
    message: message.value,
  };

  const find = conversations.value.find(
    (convo) => convo.id == selected.value.id
  );
  find.messages.push(obj);
  message.value = "";
  if (!isReceiving.value) {
    console.log(isReceiving.value);

    isReceiving.value = true;
    intervalId = setTimeout(() => {
      console.log("set");
      receiveMessage();
      isReceiving.value = false;
    }, 3000);
  }
};

const receiveMessage = () => {
  let obj = {
    sender: selected.value.sender,
    message: "Naon?",
  };

  const find = conversations.value.find(
    (convo) => convo.id == selected.value.id
  );
  find.messages.push(obj);
};
</script>
<template>
  <div
    class="w-full relative p-6 bg-white border border-gray-200 rounded-lg shadow h-3/4 my-20"
  >
    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900">
      Punten App
    </h5>
    <p class="font-normal text-gray-700 mb-2">
      Don't forget to say, 'Punten' to start a conversation!
    </p>
    <div class="border-b border-gray-200"></div>

    <div class="flex h-5/6">
      <div class="flex flex-col w-1/3 py-5">
        <ul class="divide-y divide-gray-200">
          <li
            v-for="convo in conversations"
            class="sm:pb-4 pr-5 py-3 cursor-pointer"
            @click="selected = convo"
          >
            <div class="flex items-center space-x-4 rtl:space-x-reverse">
              <div class="flex-shrink-0">
                <img
                  class="w-8 h-8 rounded-full object-cover"
                  :src="convo.image"
                  :alt="`${convo.sender} image`"
                />
              </div>
              <div class="flex-1 min-w-0">
                <p class="text-sm font-medium text-gray-900 truncate">
                  {{ convo.sender }}
                </p>
                <p class="text-sm text-gray-500 truncate">
                  {{ convo.messages[convo.messages.length - 1].sender }}:
                  {{ convo.messages[convo.messages.length - 1].message }}
                </p>
              </div>
            </div>
          </li>
        </ul>
      </div>
      <div class="w-full relative">
        <div v-if="selected" class="w-full h-full p-5 border-l relative">
          <div
            v-if="selected.type == 'group'"
            class="bg-gray-100 py-3 px-2 rounded-md mb-4 flex items-center gap-2.5"
          >
            <img
              class="w-8 h-8 object-cover rounded-full justify-start"
              :src="selected.image"
              :alt="`${selected.sender} image`"
            />
            <span class="text-sm font-semibold text-gray-900">{{
              selected.sender
            }}</span>
          </div>
          <div class="h-5/6 overflow-auto flex flex-col space-y-2">
            <div
              v-for="chat in selected.messages"
              class="flex items-start gap-2.5"
              :class="
                chat.sender == 'You' ? 'justify-end items-end' : 'justify-start'
              "
            >
              <img
                v-if="chat.sender != 'You'"
                class="w-8 h-8 object-cover rounded-full justify-start"
                :src="chat.image ?? selected.image"
                :alt="`${selected.sender} image`"
              />
              <div
                class="flex flex-col w-full max-w-[320px] leading-1.5 p-4 border-gray-200 bg-gray-100"
                :class="
                  chat.sender == 'You'
                    ? 'rounded-s-xl rounded-t-xl '
                    : 'rounded-e-xl rounded-es-xl'
                "
              >
                <div class="flex items-center space-x-2 rtl:space-x-reverse">
                  <span class="text-sm font-semibold text-gray-900">{{
                    chat.sender
                  }}</span>

                  <span
                    v-if="!chat.isFriend && selected.type == 'group'"
                    @click="modal.addFriend = true"
                    class="text-sm font-normal text-blue-500 cursor-pointer"
                    ><svg
                      xmlns="http://www.w3.org/2000/svg"
                      width="15"
                      height="15"
                      viewBox="0 0 24 24"
                      fill="none"
                      stroke="currentColor"
                      stroke-width="2"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      class="icon icon-tabler icons-tabler-outline icon-tabler-user-plus"
                    >
                      <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                      <path d="M8 7a4 4 0 1 0 8 0a4 4 0 0 0 -8 0" />
                      <path d="M16 19h6" />
                      <path d="M19 16v6" />
                      <path d="M6 21v-2a4 4 0 0 1 4 -4h4" /></svg
                  ></span>
                </div>
                <p class="text-sm font-normal py-2.5 text-gray-900">
                  {{ chat.message }}
                </p>
              </div>
              <img
                v-if="chat.sender == 'You'"
                class="w-8 h-8 rounded-full"
                src="https://i.pinimg.com/736x/22/11/d8/2211d8070ade3a24cc69dcf6883df5a5.jpg"
                :alt="`${selected.sender} image`"
                :class="chat.sender == 'You' ? 'items-end' : 'justify-start'"
              />
            </div>
          </div>

          <form
            @submit="(e) => e.preventDefault()"
            class="absolute bottom-0 left-5 right-0"
          >
            <label for="chat" class="sr-only">Your message</label>
            <div class="flex items-center px-3 py-2 rounded-lg bg-gray-100">
              <button
                type="button"
                class="inline-flex justify-center p-2 text-gray-500 rounded-lg cursor-pointer hover:text-gray-900 hover:bg-gray-100"
              >
                <svg
                  class="w-5 h-5"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 20 18"
                >
                  <path
                    fill="currentColor"
                    d="M13 5.5a.5.5 0 1 1-1 0 .5.5 0 0 1 1 0ZM7.565 7.423 4.5 14h11.518l-2.516-3.71L11 13 7.565 7.423Z"
                  />
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M18 1H2a1 1 0 0 0-1 1v14a1 1 0 0 0 1 1h16a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1Z"
                  />
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M13 5.5a.5.5 0 1 1-1 0 .5.5 0 0 1 1 0ZM7.565 7.423 4.5 14h11.518l-2.516-3.71L11 13 7.565 7.423Z"
                  />
                </svg>
                <span class="sr-only">Upload image</span>
              </button>
              <button
                type="button"
                class="p-2 text-gray-500 rounded-lg cursor-pointer hover:text-gray-900 hover:bg-gray-100"
              >
                <svg
                  class="w-5 h-5"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 20 20"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M13.408 7.5h.01m-6.876 0h.01M19 10a9 9 0 1 1-18 0 9 9 0 0 1 18 0ZM4.6 11a5.5 5.5 0 0 0 10.81 0H4.6Z"
                  />
                </svg>
                <span class="sr-only">Add emoji</span>
              </button>
              <textarea
                v-model="message"
                id="chat"
                rows="1"
                class="block mx-4 p-2.5 w-full text-sm text-gray-900 bg-white rounded-lg border border-gray-300 focus:ring-blue-500 focus:border-blue-500"
                placeholder="Your message..."
              ></textarea>
              <button
                @click="sendMessage"
                class="inline-flex justify-center p-2 text-blue-600 rounded-full cursor-pointer hover:bg-blue-100"
              >
                <svg
                  class="w-5 h-5 rotate-90 rtl:-rotate-90"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="currentColor"
                  viewBox="0 0 18 20"
                >
                  <path
                    d="m17.914 18.594-8-18a1 1 0 0 0-1.828 0l-8 18a1 1 0 0 0 1.157 1.376L8 18.281V9a1 1 0 0 1 2 0v9.281l6.758 1.689a1 1 0 0 0 1.156-1.376Z"
                  />
                </svg>
                <span class="sr-only">Send message</span>
              </button>
            </div>
          </form>
        </div>
        <div
          v-else
          class="flex w-full items-center justify-center h-full border-l"
        >
          <p>Select a chat to start conversation</p>
        </div>
        <div></div>
        <!-- Main modal -->
        <div
          id="progress-modal"
          tabindex="-1"
          aria-hidden="true"
          :class="modal.addFriend ? 'block' : 'hidden'"
          class="overflow-y-auto border overflow-x-hidden absolute flex justify-center items-center w-full md:inset-0 h-[calc(100%-1rem)] max-h-full"
        >
          <div class="relative p-4 w-full max-w-md max-h-full">
            <!-- Modal content -->
            <div class="relative bg-white rounded-lg shadow dark:bg-gray-700">
              <button
                @click="modal.addFriend = false"
                type="button"
                class="absolute top-3 end-2.5 text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm w-8 h-8 ms-auto inline-flex justify-center items-center dark:hover:bg-gray-600 dark:hover:text-white"
                data-modal-hide="progress-modal"
              >
                <svg
                  class="w-3 h-3"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  fill="none"
                  viewBox="0 0 14 14"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6"
                  />
                </svg>
                <span class="sr-only">Close modal</span>
              </button>
              <div class="p-4 md:p-5">
                <h3
                  class="mb-1 text-xl font-bold flex items-center text-gray-900 dark:text-white"
                >
                  <img
                    class="w-10 h-10 object-cover rounded-full mr-2"
                    src="https://i.pinimg.com/564x/5c/b9/78/5cb978c7b18ae304a0c5f8532a76bb0a.jpg"
                    alt="Rounded avatar"
                  />

                  +62812345678
                </h3>
                <p class="text-gray-500 dark:text-gray-400 mb-6">
                  Add this contact to your friends?
                </p>
                <p></p>
                <!-- Modal footer -->
                <div
                  class="flex items-center mt-6 space-x-4 rtl:space-x-reverse"
                >
                  <button
                    @click="addFriend"
                    type="button"
                    class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
                  >
                    Add a friend
                  </button>
                  <button
                    @click="modal.addFriend = false"
                    type="button"
                    class="py-2.5 px-5 text-sm font-medium text-gray-900 focus:outline-none bg-white rounded-lg border border-gray-200 hover:bg-gray-100 hover:text-blue-700 focus:z-10 focus:ring-4 focus:ring-gray-100 dark:focus:ring-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:border-gray-600 dark:hover:text-white dark:hover:bg-gray-700"
                  >
                    Cancel
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
