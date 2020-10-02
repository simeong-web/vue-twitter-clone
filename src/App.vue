<template>
  <div id="app" class="flex container h-screen w-full">
    <!-- side nav -->
    <div class="lg:w-1/5 border-r border-lighter px-2 lg:px-6 py-2 flex flex-col justify-between">
      <div>
        <button class="h-12 w-12 hover:bg-lightblue focus:outline-none text-3xl rounded-full text-blue">
          <i class="fab fa-twitter"></i>
        </button>
        <div>
          <button :key="`${ tab.id }`" v-for="tab in tabs" @click="id = tab.id" :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === tab.id ? 'text-blue' : ''}`">
            <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
            <p class="text-lg font-semibold text-left hidden lg:block"> {{ tab.title }} </p>
          </button>
        </div>
        <button class="text-white bg-blue rounded-full font-semibold focus:outline-none w-12 h-12 lg:h-auto lg:w-full p-3 hover:bg-darkblue">
          <p class="hidden lg:block">Tweet</p>
          <i class="fas fa-plus lg:hidden"></i>
        </button>
      </div>
      <div class="lg:w-full relative">
        <button @click="dropdown = true" class="flex items-center w-full hover:bg-lightblue rounded-full p-2 focus:outline-none">
          <img src="profile.jpg" class="w-10 h-10 rounded-full border border-lighter" />
          <div class="hidden xl:block ml-4 mr-4">
            <p class="text-sm font-bold leading-tight"> Dwayne Johnson </p>
            <p class="text-sm leading-tight"> @TheRockJohnson </p>
          </div>
          <i class="hidden xl:block fas fa-angle-down ml-auto text-lg"></i>
        </button>
        <div v-if="dropdown === true" class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-16">
          <button @click="dropdown = false" class="p-3 flex items-center w-full hover:bg-lightest p-2 focus:outline-none">
            <img src="profile.jpg" class="w-10 h-10 rounded-full border border-lighter" />
            <div class="ml-4">
              <p class="text-sm font-bold leading-tight"> Dwayne Johnson </p>
              <p class="text-sm leading-tight"> @TheRockJohnson </p>
            </div>
            <i class="fas fa-check ml-auto test-blue"></i>
          </button>
          <button @click="dropdown = false" class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none">
            Add an existing account
          </button>
          <button @click="dropdown = false" class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none">
            Log out @TheRockJohnson
          </button>
        </div>
      </div>
    </div>
    <!--- tweets section --->
    <div class="md:w-1/2 w-full md:h-full overflow-y-scroll">
      <div class="px-5 py-3 border-b border-lighter flex items-center justify-between">
        <h1 class="text-xl font-bold">Home</h1>
        <i class="fas fa-star text-xl text-blue"></i>
      </div>
      <div class="px-5 py-3 border-b-8 border-lighter flex">
        <div>
          <img src="profile.jpg" class="w-12 h-12 rounded-full">
        </div>
        <form v-on:submit.prevent = "addNewTweet" class="w-full px-4 relative">
          <textarea v-model="tweet.content" placeholder="What's up?" class="w-full mt-3 pb-3 focus:outline-none"></textarea>
          <div class="flex items-center">
            <i class="text-lg text-blue mr-4 far fa-image"></i>
            <i class="text-lg text-blue mr-4 fas fa-film"></i>
            <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
          </div>
          <button type="submit" class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none rounded-full absolute bottom-0 right-0">Tweet</button>
        </form>
      </div>
      <div class="flex flex-col-reverse">
        <div v-for="tweet in tweets" :key="`${tweet.content}`" class="w-full p-4 border-b hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img src="profile.jpg" class="h-12 w-12 rounded-full flex-none"/>
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold"> Dwayne Johnson </p>
              <p class="text-sm text-dark ml-2"> @TheRockJohnson </p>
              <p class="text-sm text-dark ml-2"> 1 sec </p>
              <i class="fas fa-angle-down text-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ tweet.content }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-retweet mr-3"></i>
                <p> 0 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-heart mr-3"></i>
                <p> 1 </p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-share-square mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-for="follow in following" :key='`${follow.name}`' class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="`${follow.src}`" class="h-12 w-12 rounded-full flex-none" alt="">
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold">{{ follow.name }}</p>
            <p class="text-sm text-dark ml-2">{{ follow.handle }}</p>
            <p class="text-sm text-dark ml-2">{{ follow.time }}</p>
            <i class="fas fa-angle-down text-dark ml-auto"></i>
          </div>
          <p class="py-2">
            {{ follow.tweet }}
          </p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <i class="far fa-comment mr-3"></i>
              <p>{{ follow.comments }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-retweet mr-3"></i>
              <p>{{ follow.retweets }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-heart mr-3"></i>
              <p>{{ follow.likes }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-share-square mr-3"></i>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--- trending section --->
    <div class="md:w-1/3 md:h-full w-0 h-0 md:block hidden border-l border-lighter py-2 px-6 overflow-y-scroll relative">
      <input type="text" class="pl-12 text-sm rounded-full w-full p-2 bg-lighter focus:outline-none" placeholder="Search Vue-Twitter">
      <i class="fas fa-search absolute left-0 top-0 mt-5 ml-12 text-sm text-light"></i>
      <div class="w-full rounded-lg bg-lightest">
        <div class="flex items-center justify-between p-3 mt-5">
          <p class="text-lg font-bold">Trends for You</p>
          <div class="i fas fa-cog text-lg text-blue"></div>
        </div>
        <button v-for="trend in trending" :key="`${trend.id}`" class="w-full flex justify-between hover:bg-lighter focus:outline-none p-3 border-t border-lighter">
          <div>
            <p class="text-sm text-left leading-tight text-dark">{{ trend.top }}</p>
            <p class="font-bold text-left leading-tight">{{ trend.title }}</p>
            <p class="text-left leading-tight text-dark">{{ trend.bottom }}</p>
          </div>
          <i class="fas fa-angle-down text-lg text-dark"></i>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter focus:outline-none">
          Show More
        </button>
      </div>
      <div class="w-full rounded-lg bg-lightest my-4">
        <div class="p-3 mt-5">
          <p class="text-lg font-bold">Who to Follow</p>
        </div>
        <button v-for="friend in friends" :key="`${friend.name}`" class="w-full flex hover:bg-lighter focus:outline-none p-3 border-t border-lighter">
          <img :src="`${friend.src}`" class="w-12 h-12 rounded-full border border-lighter" />
          <div class="hidden lg:block ml-4">
            <p class="text-sm text-left font-bold leading-tight"> {{ friend.name }} </p>
            <p class="text-sm leading-tight"> {{ friend.handle }} </p>
          </div>
          <button class="text-sm font-bold text-blue py-2 px-4 rounded-full border-2 border-blue ml-auto focus:outline-none">
            Follow
          </button>
        </button>
        <button class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter focus:outline-none">
          Show More
        </button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'app',
    data () {
      return {
        tabs: [
          {icon: 'fas fa-home', title: 'Home', id: 'home'},
          {icon: 'fas fa-hashtag', title: 'Explore', id: 'explore'},
          {icon: 'fas fa-bell', title: 'Notifications', id: 'notifications'},
          {icon: 'fas fa-envelope', title: 'Messages', id: 'messages'},
          {icon: 'fas fa-bookmark', title: 'Bookmarks', id: 'bookmarks'},
          {icon: 'fas fa-clipboard-list', title: 'Lists', id: 'lists'},
          {icon: 'fas fa-user', title: 'Profile', id: 'profile'},
          {icon: 'fas fa-ellipsis-h', title: 'More', id: 'more'}
        ],
        id: 'home',
        dropdown: false,
        trending: [
          {top: 'Trending in TX', title: 'Gigi', bottom: 'Trending with: Rip Gigi', id: '1'},
          {top: 'Music', title: 'We Won', bottom: '135k Tweets', id: '2'},
          {top: 'Pop', title: 'Blue Ivy', bottom: '40k Tweets', id: '3'},
          {top: 'Trending in US', title: 'Denim Day', bottom: '38k Tweets',  id: '4'},
          {top: 'Trending', title: 'When Beyonce', bottom: '25.4k Tweets', id: '5'}
        ],
        friends: [
          {src: 'hulk-hogan.jpg', name: 'Hulk Hogan', handle: '@RealHulk'},
          {src: 'randy.jpg', name: 'Randy Orton', handle: '@RandyBabe'},
          {src: 'kane.jpg', name: 'Kane', handle: '@KaneWWE'}
        ],
        following: [
          {src: 'hulk-hogan.jpg', name: 'Hulk Hogan', handle: '@RealHulk', time: '20 min', tweet: 'I\'ve been keeping busy, taking the trash out @RandyBabe *wink*', comments: '1,564', retweets: '1,230', likes: '1,000,152'},
          {src: 'randy.jpg', name: 'Randy Orton', handle: '@RandyBabe', time: '55 min', tweet: 'I will RKO you out of nowhere @RealHulk!', comments: '1,464', retweets: '1,030', likes: '980,155'},
          {src: 'kane.jpg', name: 'Kane', handle: '@KaneWWE', time: '1.4 hr', tweet: 'Hulk Hogan is the best hands-down!', comments: '1,164', retweets: '2030', likes: '1,100,151'},
          {src: 'hulk-hogan.jpg', name: 'Hulk Hogan', handle: '@RealHulk', time: '20 min', tweet: 'I\'ve been keeping busy, taking the trash out @RandyBabe *wink*', comments: '1,564', retweets: '1,230', likes: '1,000,152'},
          {src: 'randy.jpg', name: 'Randy Orton', handle: '@RandyBabe', time: '55 min', tweet: 'I will RKO you out of nowhere @RealHulk!', comments: '1,464', retweets: '1,030', likes: '980,155'},
          {src: 'kane.jpg', name: 'Kane', handle: '@KaneWWE', time: '1.4 hr', tweet: 'Hulk Hogan is the best hands-down!', comments: '1,164', retweets: '2030', likes: '1,100,151'}
        ],
        tweets: [
          {content: 'This is a test tweet.'}
        ],
        tweet: {
          content: ''
        }
      }
    },
    methods: {
      addNewTweet () {
        let newTweet = {
          content: this.tweet.content
        };
        this.tweets.push(newTweet);
        this.tweet.content = '';
      }
    }
  }
</script>