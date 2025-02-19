<script setup lang="ts">
import { computed } from 'vue'

import { useRequestStore } from '../../../stores/requestStore'
import { CollapsibleSection } from '../../CollapsibleSection'
// import Copilot from './Copilot.vue'
import { SimpleGrid } from '../../Grid'
import ResponseBody from './ResponseBody.vue'
import ResponseHeaders from './ResponseHeaders.vue'
import ResponseMetaInformation from './ResponseMetaInformation.vue'

const { activeResponse, activeRequestId } = useRequestStore()

// Headers
const responseHeaders = computed(() => {
  const headers = activeResponse.value?.headers
  if (!headers) return []
  return Object.keys(headers).map((key) => ({ name: key, value: headers[key] }))
})

// Cookies
const responseCookies = computed(() => {
  const cookies = activeResponse.value?.cookies

  return cookies
    ? Object.keys(cookies).map((key) => ({ name: key, value: cookies[key] }))
    : []
})

// Pretty print JSON
const responseData = computed(() => activeResponse.value?.data)
</script>
<template>
  <div class="scalar-api-client__main__right custom-scroll">
    <div class="scalar-api-client__main__content">
      <label>Response</label>
      <div class="meta">
        <template v-if="activeRequestId && activeResponse">
          <ResponseMetaInformation :response="activeResponse" />
        </template>
        <template v-else>
          <div class="meta-item">
            <span>Send your first request to start</span>
          </div>
        </template>
      </div>
    </div>
    <div>
      <ResponseBody
        :active="!!activeResponse"
        :data="responseData"
        :headers="responseHeaders" />
      <ResponseHeaders :headers="responseHeaders" />
      <CollapsibleSection title="Cookies">
        <SimpleGrid
          v-show="responseCookies.length > 0"
          :items="responseCookies" />
        <template v-if="responseCookies.length === 0">
          <div class="scalar-api-client__empty-state">No Cookies</div>
        </template>
      </CollapsibleSection>
      <div class="scalar-api-client__main__scroll-container" />
    </div>
  </div>
</template>
<style>
.scalar-api-client__main__right {
  width: 50%;
  padding: 0 6px 12px 18px;
}
@media screen and (max-width: 820px) {
  .scalar-api-client__main__right {
    width: 100%;
    border-right: none;
    padding: 0 0 12px 12px;
  }
}
.scalar-api-client__main__right :deep(.scalar-copilot__header-button) {
  position: absolute;
  top: 6px;
  right: 12px;
}
</style>
