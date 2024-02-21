<script setup>
import { ref, onMounted, watch } from 'vue'

const props = defineProps({
  "message": String,
  "comment": {
    'type': Object,
    'default': { 'cmt': "", 'truth': false }
  }
});

const emit = defineEmits(["wait"]);

const msg = ref("")
const typing = ref(false)
const currentComment = ref("")
var charIndex = 0
var typeValue = ""
const typingSpeed = 15
// const typingSpeed = 0

onMounted(() => {
  function typeText() {
    if (charIndex < props.message.length) {
      typing.value = true
      var newChar = props.message.charAt(charIndex);
      typeValue += newChar;
      msg.value = typeValue + "...";
      charIndex++;
      setTimeout(typeText, typingSpeed);
    } else {
      typing.value = false
      msg.value = props.message
    }
  }
  typeText()
  // msg.value = props.message
  // emit("wait", false)
})

watch(typing, (newVal) => {
  emit("wait", newVal)
})

watch(props, (newProps) => {
  let newCmt = newProps.comment.cmt
  if (newCmt != currentComment.value) {
    var cmtIndex = 0
    var cmtTypeValue = ""
    function typeComment() {
      if (cmtIndex < newCmt.length) {
        var newCmtChar = newCmt.charAt(cmtIndex);
        cmtTypeValue += newCmtChar;
        currentComment.value = cmtTypeValue + "...";
        cmtIndex++;
        setTimeout(typeComment, typingSpeed);
      } else {
        currentComment.value = newCmt
      }
    }
    typeComment()
  }
})
</script>

<template>
  <table class="m-box">
    <tr class="meta">
      <td class="gigaSVG"><svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24">
          <path fill="currentColor"
            d="M7 2a6 6 0 0 0-6 6a6 6 0 0 0 6 6a6 6 0 0 0 6-6a6 6 0 0 0-6-6m14.5 4A1.5 1.5 0 0 0 20 7.5A1.5 1.5 0 0 0 21.5 9A1.5 1.5 0 0 0 23 7.5A1.5 1.5 0 0 0 21.5 6M17 8a3 3 0 0 0-3 3a3 3 0 0 0 3 3a3 3 0 0 0 3-3a3 3 0 0 0-3-3m0 7a3.5 3.5 0 0 0-3.5 3.5A3.5 3.5 0 0 0 17 22a3.5 3.5 0 0 0 3.5-3.5A3.5 3.5 0 0 0 17 15" />
        </svg></td>
      <td><span> Giga:</span></td>
    </tr>
    <tr class="msg">
      <td></td>
      <td><span v-html="msg"></span></td>
    </tr>
    <tr v-if="currentComment">
      <td v-if="props.comment.truth" class="cmtSVG">
        <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 16 16">
          <path fill="currentColor"
            d="M15 5.5a4.5 4.5 0 1 1-9 0a4.5 4.5 0 0 1 9 0m-2.146-1.854a.5.5 0 0 0-.708 0L9.5 6.293l-.646-.647a.5.5 0 1 0-.708.708l1 1a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0 0-.708M3.5 3h2.1c.183-.358.404-.693.657-1H3.5A2.5 2.5 0 0 0 1 4.5v5A2.5 2.5 0 0 0 3.5 12H4v1.942a.98.98 0 0 0 1.625.738L8.688 12H12.5A2.5 2.5 0 0 0 15 9.5v-.837c-.29.411-.634.78-1.023 1.098A1.5 1.5 0 0 1 12.5 11H8.312L5 13.898V11H3.5A1.5 1.5 0 0 1 2 9.5v-5A1.5 1.5 0 0 1 3.5 3" />
        </svg>
      </td>
      <td v-else class="cmtSVG">
        <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 16 16">
          <path fill="currentColor"
            d="M5.6 3H3.5A1.5 1.5 0 0 0 2 4.5v5A1.5 1.5 0 0 0 3.5 11H5v2.898L8.312 11H12.5a1.5 1.5 0 0 0 1.477-1.239A5.531 5.531 0 0 0 15 8.664V9.5a2.5 2.5 0 0 1-2.5 2.5H8.688l-3.063 2.68A.98.98 0 0 1 4 13.942V12h-.5A2.5 2.5 0 0 1 1 9.5v-5A2.5 2.5 0 0 1 3.5 2h2.757A5.508 5.508 0 0 0 5.6 3m4.9 7a4.5 4.5 0 1 0 0-9a4.5 4.5 0 0 0 0 9M10 3.5a.5.5 0 0 1 1 0v2a.5.5 0 0 1-1 0zm1.125 4a.625.625 0 1 1-1.25 0a.625.625 0 0 1 1.25 0" />
        </svg>
      </td>
      <td style="padding: 5px 0;">{{ currentComment }}</td>
    </tr>
  </table>
</template>

<style scoped>
.m-box {
  margin-top: 25px;
}

.gigaSVG {
  font-size: 22px;
}

.meta {
  color: var(--ice-blue);
  font-size: 14px;
  font-weight: lighter;
}

.msg {
  font-size: 16px;
}

.cmtSVG {
  font-size: 15px;
}
</style>