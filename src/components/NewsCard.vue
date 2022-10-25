<template>
    <div class="news-card">
        <img class="news-card__image" :src="image" :alt="slug" />
        <div class="news-card__info">
            <h1 class="news-card__header">{{ title }}</h1>
            <div class="news-card__date" :class="color">
                {{ datePublish }}
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'news-card',
  props: {
    title: String,
    image: String,
    shortText: String,
    datePublish: String,
    slug: String,
    color: String,
  },
});

</script>

<style lang="scss">
.news-card {
    display: flex;
    flex-direction: column;
    width: 264px;
    background-color: #F5F5F5;
    border-radius: 16px;
    overflow: hidden;
    height: 366px;
    justify-content: space-between;

    @media screen and (max-width: 1347px) {
        width: 231px;
    }

    @media screen and (max-width: 480px) {
        width: 210px;
    }

    &__image {
        max-height: 217px;
    }

    &__info {
        padding: 22px 24px 25px;
    }

    &__header {
        font-family: 'Roboto';
        font-weight: 700;
        font-size: 18px;
        line-height: 130%;
        color: #000;
        width: 100%;
        height: 69px;
        margin: 0 0 11px;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        overflow: hidden;
    }

    &__date {
        width: 100%;
        display: flex;
        justify-content: space-between;
        font-family: 'Roboto';
        font-weight: 700;
        font-size: 18px;
        line-height: 120%;

        &::before {
            width: 21px;
            height: 21px;
            content: '';
            mask-repeat: no-repeat;
        }

        &.green {
            color: #378B60;

            &::before {
                mask-image: url(../assets/images/pig.svg);
                background-color: #378B60;
            }
        }

        &.pink {
            color: #FF00B8;

            &::before {
                mask-image: url(../assets/images/flash.svg);
                background-color: #FF00B8;
            }
        }

        &.orange {
            color: #FB9600;

            &::before {
                mask-image: url(../assets/images/fire.svg);
                background-color: #FB9600;
            }
        }
    }
}
</style>
