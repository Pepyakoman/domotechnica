<template>
  <div class="page">
    <div class="search">
      <input
        class="search__input"
        placeholder="Поиск"
        @keyup="search"
        type="text"
        aria-label="Search"
      >
    </div>
    <div class="news">
      <news-card v-for="(cardData, index) in news" :key="index" v-bind="cardData"></news-card>
    </div>
    <footer class="footer">
      <button class="button button__green" @click="uploadPage(random(), 'green')">
        Загрузить
      </button>
      <button class="button button__pink" @click="uploadPage(random(), 'pink')">
        Загрузить
      </button>
      <button class="button button__orange" @click="uploadPage(random(), 'orange')">
        Загрузить
      </button>
    </footer>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';
import NewsCard from './NewsCard.vue';

type News = {
    title: string;
    image: string;
    shortText: string;
    datePublish: string;
    slug: string;
    color: string;
    number: number;
};

/** Компонент страницы с новостями */
export default defineComponent({
  name: 'news-page',
  components: {
    NewsCard,
  },
  data() {
    return {
      news: [] as Array<News>,
      uploadedNews: [] as Array<News>,
    };
  },
  mounted() {
    this.uploadPage(1, 'green');
  },
  methods: {
    /** Загрузка данных страницы */
    async uploadPage(uploadPageNumber: number, color: string) {
      return axios.get('https://domotekhnika.ru/api/v1/news', {
        params: {
          page: uploadPageNumber,
        },
      }).then((res) => {
        const { news } = res.data.data;
        this.formNews(news, uploadPageNumber, color);
      });
    },

    /** Сформировать новости */
    formNews(uploadNews: Array<News>, pageNumber: number, color: string) {
      if (uploadNews.length) {
        const existPage = this.news.find((data: News) => data.number === pageNumber);
        if (existPage) {
          if (existPage.color !== color) {
            this.changeColor(pageNumber, color);
          }
        }
        else {
          this.addNews(uploadNews, pageNumber, color);
        }
      }
    },

    /** Сменить цвет */
    changeColor(number: number, color: string) {
      this.news.forEach((data: News) => {
        if (data.number === number) data.color = color;
      });
    },

    /** Добавить новости */
    addNews(uploadNews: Array<News>, number: number, color: string) {
      this.uploadedNews.push(...uploadNews
        .filter((data: News) => data.image)
        .map((data: News) => {
          data.color = color;
          data.number = number;
          return data;
        }),
      );
      this.uploadedNews.sort((a: News, b: News) => a.number - b.number);
      this.news = this.uploadedNews;
    },

    /** Случайное число от 1 до 10 */
    random() {
      return Math.floor(Math.random() * 10) + 1;
    },

    /** Поиск по массиву загруженных новостей */
    search(event: KeyboardEvent) {
      const target = event.target as HTMLInputElement;
      if (target.value === '') {
        this.news = this.uploadedNews;
      }
      else {
        this.news = this.uploadedNews.filter((news: News) => {
          return news.title.toLowerCase().includes(target.value.toLowerCase());
        });
      }
    },
  },
});
</script>

<style scoped lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@500&display=swap');

.page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 1416px;
  margin: 0 auto;
  padding: 0 48px;
  box-sizing: content-box;

  @media screen and (max-width: 1347px) {
    width: 1251px;
  }

  @media screen and (max-width: 1092px) {
    width: 996px;
  }

  @media screen and (max-width: 837px) {
    width: 741px;
  }

  @media screen and (max-width: 582px) {
    width: 486px;
  }

  @media screen and (max-width: 480px) {
    width: 432px;
    padding: 0 24px;
  }

  .search {
    display: flex;
    flex-direction: row;
    justify-content: right;
    align-items: center;
    width: 100%;

    &::after {
      content: url('../assets/images/lupa.svg');
      width: 22px;
      height: 22px;
      position: absolute;
      margin-right: 19px;
    }

    &__input {
      margin: 48px 0;
      border: 2px solid #4D4D4D;
      border-radius: 12px;
      height: 45px;
      padding: 16px 42px 16px 18px;
      width: 100%;

      &::placeholder {
        color: #4D4D4D;
        font-family: 'Roboto';
        font-size: 16px;
        line-height: 130%;
      }
    }
  }

  .news {
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    gap: 24px;

    @media screen and (max-width: 480px) {
      gap: 12px;
    }
  }

  .footer {
    display: flex;
    gap: 29px;
    margin: 24px 0 74px;
    justify-content: center;

    @media screen and (max-width: 582px) {
      gap: 12px;
    }

    @media screen and (max-width: 480px) {
      gap: 18px 8px;
      display: grid;
      grid-template-columns: 1fr 1fr;
    }

    .button {
      color: #fff;
      width: 175px;
      height: 56px;
      padding: 16px 32px;
      border-radius: 36px;
      border: none;
      display: flex;
      flex-direction: row;
      justify-content: center;
      align-items: center;
      cursor: pointer;

      @media screen and (max-width: 582px) {
        width: 154px;
        height: 56px;
      }

      @media screen and (max-width: 480px) {
        width: 100%;

        &:last-child {
          grid-column: 1 / 3;
        }
      }

      &::after {
        filter: invert(1);
        width: 18px;
        height: 18px;
        margin-left: 13px;
      }

      &__green {
        background: #378B60;

        &::after {
          content: url(../assets/images/pig.svg);
        }
      }

      &__pink {
        background: #FF00B8;

        &::after {
          content: url(../assets/images/flash.svg);
        }
      }

      &__orange {
        background: #FB9600;

        &::after {
          content: url(../assets/images/fire.svg);
        }
      }
    }
  }
}
</style>
