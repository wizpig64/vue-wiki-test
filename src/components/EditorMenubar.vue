<template>
  <div class="menubar" v-if="nodes && marks">
    <button class="menubar__button"
            :class="{ 'is-active': marks.bold.active() }"
            @click="marks.bold.command">
      <fa icon="bold"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': marks.italic.active() }"
            @click="marks.italic.command">
      <fa icon="italic"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': marks.strike.active() }"
            @click="marks.strike.command">
      <fa icon="strikethrough"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': marks.underline.active() }"
            @click="marks.underline.command">
      <fa icon="underline"/>
    </button>
    <button class="menubar__button"
            @click="marks.code.command"
            :class="{ 'is-active': marks.code.active() }">
      <fa icon="code"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.paragraph.active() }"
            @click="nodes.paragraph.command">
      <fa icon="paragraph"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.heading.active({ level: 1 }) }"
            @click="nodes.heading.command({ level: 1 })">
      <fa icon="heading"/>1
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.heading.active({ level: 2 }) }"
            @click="nodes.heading.command({ level: 2 })">
      <fa icon="heading"/>2
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.heading.active({ level: 3 }) }"
            @click="nodes.heading.command({ level: 3 })">
      <fa icon="heading"/>3
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.bullet_list.active() }"
            @click="nodes.bullet_list.command">
      <fa icon="list-ul"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.ordered_list.active() }"
            @click="nodes.ordered_list.command">
      <fa icon="list-ol"/>
    </button>
    <button class="menubar__button"
            :class="{ 'is-active': nodes.code_block.active() }"
            @click="nodes.code_block.command">
      <fa icon="code"/>
    </button>
    <form class="menubar__form"
          v-if="linkMenuIsActive"
          @submit.prevent="setLinkUrl(linkUrl, marks.link, focus)">
      <input class="menubar__input"
             type="text" v-model="linkUrl"
             placeholder="https://" ref="linkInput"
             @keydown.esc="hideLinkMenu"/>
      <button class="menubar__button"
              @click="setLinkUrl(null, marks.link, focus)"
              type="button">
        <fa icon="times-circle"/>
      </button>
    </form>
    <button v-else
            class="menubar__button"
            @click="showLinkMenu(marks.link)"
            :class="{ 'is-active': marks.link.active() }">
      <fa icon="link"/>
    </button>
  </div>
</template>

<script>
export default {
  name: 'EditorMenubar',
  props: ['marks', 'nodes', 'focus'],
  data () {
    return {
      linkUrl: null,
      linkMenuIsActive: false
    }
  },
  methods: {
    showLinkMenu (type) {
      this.linkUrl = type.attrs.href
      this.linkMenuIsActive = true
      this.$nextTick(() => {
        this.$refs.linkInput.focus()
      })
    },
    hideLinkMenu () {
      this.linkUrl = null
      this.linkMenuIsActive = false
    },
    setLinkUrl (url, type, focus) {
      type.command({ href: url })
      this.hideLinkMenu()
      focus()
    }
  }
}
</script>

<style scoped lang="scss">
  $color-black: #3E3F3A;
  $color-white: #FFFFFF;

  .menubar {

    display: flex;
    margin-bottom: 1rem;
    transition: visibility 0.2s 0.4s, opacity 0.2s 0.4s;

    &.is-hidden {
      visibility: hidden;
      opacity: 0;
    }

    &.is-focused {
      visibility: visible;
      opacity: 1;
      transition: visibility 0.2s, opacity 0.2s;
    }

    &__form {
      display: flex;
      align-items: center;
    }

    &__input {
      font: inherit;
      border: none;
      background: transparent;
    }

    &__button {
      font-weight: bold;
      display: inline-flex;
      background: transparent;
      border: 0;
      color: $color-black;
      padding: 0.2rem 0.5rem;
      margin-right: 0.2rem;
      border-radius: 3px;
      cursor: pointer;

      &:hover {
        background-color: $color-black;
        color: $color-white;
      }

      &.is-active {
        background-color: $color-black;
        color: $color-white;
      }
    }
  }
</style>
