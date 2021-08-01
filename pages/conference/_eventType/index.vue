<template>
    <i18n-page-wrapper class="px-8 sm:px-10 md:px-32 lg:px-52" custom-x>
        <core-h1 :title="$t(eventType ? `${eventType}.title` : '')"></core-h1>
        <i18n
            :path="eventType ? `${eventType}.intro` : ''"
            tag="p"
            class="intro"
        ></i18n>
        <speech-card-collection>
            <speech-card
                v-for="speech in speechesData"
                :id="speech.id"
                :key="`speech_${speech.id}`"
                :title="speech.title"
                :category="speech.category"
                :speakers="speech.speakers"
                :to="`/conference/${speech.event_type}/${speech.id}/`"
            >
            </speech-card>
        </speech-card-collection>
    </i18n-page-wrapper>
</template>

<script>
import i18n from '@/i18n/conference/speeches.i18n'
import I18nPageWrapper from '@/components/core/i18n/PageWrapper'
import CoreH1 from '@/components/core/titles/H1'
import SpeechCard from '@/components/events/SpeechCard'
import SpeechCardCollection from '@/components/events/SpeechCardCollection'

export default {
    i18n,
    name: 'PageConferenceSpeeches',
    components: {
        I18nPageWrapper,
        CoreH1,
        SpeechCard,
        SpeechCardCollection,
    },
    asyncData({ redirect, params }) {
        const eventType = params.eventType
        if (!['talks', 'tutorials'].includes(eventType)) {
            redirect('/')
        }
    },
    data() {
        return {
            eventType: '',
            speechesData: [],
        }
    },
    async mounted() {
        this.eventType = this.$route.params.eventType
        await this.$store.dispatch('$getSpeechesData', this.eventType)
        this.speechesData = this.$store.state.speechesData
    },
}
</script>

<style lang="postcss" scoped>
.intro {
    @apply text-xs md:text-sm mb-8;
    line-height: 33px;
}
</style>