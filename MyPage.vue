<template>
  <div class="ui main container"> 
    <div class="ui segment">
      <p>ようこそ、{{userName}}さん</p>
    </div>
  </div>

  
    <div class="ui main container">  
            <div class="ui segment">
              <h1>今日のおすすめメニュー</h1>
              
              <div class="content">
                <h4>{{ recipe.recipeTitle }}</h4>
                <p>年齢: {{ recipe.age }}歳向け</p>
                <p>総カロリー: {{ recipe.kcal }}kcal</p>
                <p>
                  {{ recipe.recipeContent }}
                </p>
                <div class="ui divider"></div>
              </div>
            
            </div>
    </div>
    
    <!-- 見える部分だけ実装 -->
    <div class="ui main container">  
      <div class="ui segment">
        <form class="ui large form">
          <h2>食事登録</h2>
          
          
         <div class="field">
           <div class="ui input">
             <input type="date" placeholder="日にち">
           </div>
          </div>
  
          <div class="field">        
          <div class="ui input">
             <input type="" placeholder="ごはん">
           </div>
          </div>
          
          <div class="field">
           <div class="ui input">
             <input type="" placeholder="カロリー">
           </div>
          </div>
          
          <button class="ui huge green fluid button" type="submit">
            登録
          </button>
        </form>
      </div>
    </div>
    
    <div class="ui main container">  
      <div class="ui segment">
        <form class="ui large form">
          <h2>記録確認</h2>
        </form>
        
        <div class="field">
           <div class="ui input">
             <input v-model="searchDay" type="date" placeholder="">
           </div>
          </div>
          
        <div class="field">
          <p>朝ご飯</p>
          <p>{{ dish[0].kcal }}キロカロリー</p>
          <p>昼ご飯</p>
          <p>{{ dish[0].kcal }}キロカロリー</p>
          <p>夜ご飯</p>
          <p>{{ dish[0].kcal }}キロカロリー</p>
        </div>
          
      </div>
    </div>
    
    <div class="ui main container">  
      <button class="ui grey fluid button" type="submit">
            退会
      </button>
    </div>

</template>

<script>
// 必要なものはここでインポートする
// @は/srcの同じ意味です
// import something from '@/components/something.vue';
import { baseUrl } from '@/assets/config.js';

export default {
  name: 'MyPage',

  components: {
    // 読み込んだコンポーネント名をここに記述する
  },

  data() {
    // Vue.jsで使う変数はここに記述する
    return {
      recipe: [],
      dish: [],
      userName:null,
    };
  },

  computed: {
    // 計算した結果を変数として利用したいときはここに記述する
    
    
  },
  
  created: async function () {
    // Vue.jsの読み込みが完了したときに実行する処理はここに記述する
    this.userName=window.localStorage.getItem('userName');
    this.userId=window.localStorage.getItem('userId');
    // apiからarticleを取得する
    await this.getRecipe();
    await this.getDish();
  },

  methods: {
    // Vue.jsで使う関数はここで記述する
    
     async getRecipe() {

      try {
        /* global fetch */
        const res = await fetch(baseUrl + "/recipe", {
          method: "GET",
        });

        const text = await res.text();
        const jsonData = text ? JSON.parse(text) : {};
        console.log(jsonData);
        // fetchではネットワークエラー以外のエラーはthrowされないため、明示的にthrowする
        if (!res.ok) {
          const errorMessage =
            jsonData.message ?? "エラーメッセージがありません";
          throw new Error(errorMessage);
        }

        // 記事がなかった場合undefinedとなり、記事追加時のunshiftでエラーとなるため、空のarrayを代入
        this.recipe = jsonData.articles ?? [];
        //console.log(this.recipe);
      } catch (e) {
        
      }
    },
    
     async getDish() {
       let tmp = ``;
       tmp += `userId=`;
       tmp += `fujikiID`;
       tmp += `&dishkind=昼`;
       tmp += `&date=20230719`;
       
      
      
      const qs = tmp;
      console.log(qs)

      try {
        /* global fetch */
        const res = await fetch(baseUrl + `/dish?${qs}`, {
          method: "GET",
        });

        const text = await res.text();
        const jsonData = text ? JSON.parse(text) : {};
        console.log(jsonData);
        // fetchではネットワークエラー以外のエラーはthrowされないため、明示的にthrowする
        if (!res.ok) {
          const errorMessage =
            jsonData.message ?? "エラーメッセージがありません";
          throw new Error(errorMessage);
        }

        // 記事がなかった場合undefinedとなり、記事追加時のunshiftでエラーとなるため、空のarrayを代入
        this.dish = jsonData.dish ?? [];
        console.log(this.dish);
      } catch (e) {
        
      }
    },
  }
}
</script>

<style scoped>
/* このコンポーネントだけに適用するCSSはここに記述する */

</style>
