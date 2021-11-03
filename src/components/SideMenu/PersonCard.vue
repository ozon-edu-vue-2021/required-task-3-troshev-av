<template>
  <div class="person">
    <div class="person__photo">
      <img :src="person.picture" alt="photo" />
    </div>
    <div class="person__info">
      <div class="person__name person__info-row">
        <b>{{ person.name }}</b>
      </div>

      <div class="person__about person__info-row">{{ person.about }}</div>
      <div class="person__group person__info-row">{{ person.group }}</div>
    </div>
    <div class="person__actions">
      <UiButton :href="mailLink" target="_blank" rel="noreferrer nofollow">
        <MailIcon />
      </UiButton>

      <UiButton
        :href="person.picture"
        target="_blank"
        rel="noreferrer nofollow"
      >
        КЗ
        <template #append>
          <LinkIcon />
        </template>
      </UiButton>
    </div>
  </div>
</template>

<script>
import LinkIcon from "../Icons/LinkIcon";
import MailIcon from "../Icons/MailIcon";
import UiButton from "../Ui/UiButton";

export default {
  name: "PersonCard",
  components: {
    LinkIcon,
    MailIcon,
    UiButton,
  },
  props: {
    person: {
      type: Object,
      default: null,
    },
  },
  computed: {
    mailLink() {
      return `mailto:${this.person.email}`;
    },
  },
};
</script>

<style scoped>
.person {
  display: grid;
  grid-template: "avatar info" 1fr "avatar actions" auto;
  grid-gap: 16px;
}

.person__photo {
  grid-area: avatar;
}

.person__photo img {
  height: 160px;
  width: 160px;
  vertical-align: middle;
}

.person__info {
  grid-area: info;
  overflow: hidden;
}

.person__info-row:nth-child(1n + 2) {
  margin-top: 4px;
}

.person__name {
  font-size: 1.25em;
  color: hsl(var(--primary-hsl));
}

.person__about {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.person__group {
  color: hsl(var(--gray-hsl));
}

.person__actions {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
