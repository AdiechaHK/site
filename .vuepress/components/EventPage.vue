<template>
  <div class="event">
    <header>
      <h1 class="title">
        <!-- <a :href="'https://meetup.com/vue-bangalore/events/' + $page.frontmatter.meetup" target="_blank"> -->
          <span class="title-hashtag">VueRJT #{{ $page.frontmatter.id }}</span>
      <!--   </a> -->
        <span class="title-text">{{ $page.frontmatter.title }}</span>
      </h1>
    </header>

    <main class="agenda">
      <EventGallery :photos="$page.frontmatter.photos" />

      <h2>Agenda</h2>
      <ul>
        <li v-for="(item, index) in $page.frontmatter.agenda" :key="index">
          <component :is="use(item.type)" v-bind="item" />
        </li>
      </ul>
    </main>

    <footer>
      <section v-if="date">
        <h2>Details</h2>

        <p><strong>Date:</strong> {{ date.toDateString() }}</p>
        <p><strong>Time:</strong> {{ $page.frontmatter.time }}</p>
        <p><strong>Venue:</strong> {{ $page.frontmatter.venue.name }} (<a :href="$page.frontmatter.venue.map" target="_blank" rel="noopener noreferrer">see on map</a>)</p>
      </section>

      <section>
        <h2>Register</h2>
        <iframe :src="$page.frontmatter.register" frameborder="10" height="600" width="100%"></iframe>
      </section>

      <!-- <h2>Sponsors</h2>
      <EventSponsor v-for="(item, index) in $page.frontmatter.sponsors" :key="item.sponsor" v-bind="item" /> -->

      <!-- <h2>Organizers</h2>
      <EventSpeaker v-for="(item, index) in $page.frontmatter.organizers" :key="item" :speaker="item" /> -->
      <h2>Anchoring</h2>
      <EventSpeaker v-for="(item, index) in $page.frontmatter.anchoring" :key="item" :speaker="item" />
    </footer>
  </div>
</template>

<script>
export default {
  methods: {
    use (type) {
      switch (type) {
        case 'talk': return 'EventTalk';
        case 'show': return 'EventShow';
        case 'tell': return 'EventTell';
        case 'workshop': return 'EventWorkshop';
        case 'Q&A': return 'EventQuestion';
        default: return 'EventDefault';
      }
    }
  },
  computed: {
    date() {
      return this.$page.frontmatter.date ? new Date(this.$page.frontmatter.date) : null
    },
    widget() {
      return this.$page.frontmatter.townscript ? "https://www.townscript.com/widget/" + this.$page.frontmatter.townscript : null;
    }
  }
}
</script>

<style scoped>
.title-hashtag {
  opacity: .75;
  display: inline-block;
  margin-right: 1rem;
}
</style>


