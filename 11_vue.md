

# 11_vue

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <div v-for="movie in movies">
            <h3>{{movie.title}} : {{movie.genre}}</h3>
            {{movie.poster_url}}
            <input type="text" v-model="movie.newScore" @keyup.enter="addScore(movie)">
            <li v-for="score in movie.score_set">{{score.content}} | {{score.score}}점</li>
            <!-- <h3>{{movie.title}}</h3> -->
        </div>
    </div>
    
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://unpkg.com/axios/dist/axios.min.js"></script>

    <script>
        const MY_SERVER = "http://homeworkshop-jyyt0147.c9users.io"
        const app = new Vue({
            el:"#app",
            data:{
                movies:[]
            },
            mounted:function() {
                axios.get(`${MY_SERVER}/api/v1/movies`)
                // 성공 시
                // 에러뜨면 강력새로고침도 해보자
                .then((r) => {
                    console.log(r)
                    const moviess = r.data
                    // newScore 가 추가됨
                    // {titlle:'', genre:'', }  에서
                    // {titlle:'', genre:'', newScore: ''} 로
                    this.movies = moviess.map((movie) => {
                        return {...movie, newScore:''}

                    })
                })
                // 실패 시
                .catch((e) => {
                    console.log(e)
                })
            },
            methods:{
                // 댓글 작성
                addScore:function(movie) {
                    axios.post(`${MY_SERVER}/api/v1/movies/${movie.id}/scores/`, {content:movie.newScore, score:1})
                    // 성공 시
                    .then((r) => {
                        console.log(r)
                        const score = r.data.data
                        movie.score_set.push(score)
                        movie.newScore = ""
                    })
                    // 실패 시
                    .catch(() => {

                    })
                },
            },
        })
    
    </script>
</body>
</html>
```

