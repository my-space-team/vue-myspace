<template>
    <!-- ***** Header Area Start ***** -->
    <header class="header-area header-sticky">
    <div class="container">
      <div class="row">
        <div class="col-12">
          <nav class="main-nav">
            <!-- ***** Logo Start ***** -->
            <router-link to="/" class="logo" >
              <img src="@/assets/images/logo-myspace-blue.png" alt="" style="height: 50px; margin-top: 15px;"/>
            </router-link>
            <!-- ***** Logo End ***** -->
            <!-- ***** Search End ***** -->
            <div class="search-input">
              <form id="search">
                <!-- <div> -->
                  <div class="search-img">
                    <img src="@/assets/images/search_icon.png" />
                  </div>
                  <div>
                    <input type="text" placeholder="쇼핑검색" id="searchText" name="searchKeyword"  v-model="searchKeyword"  @keyup.enter="search" />
                  </div>
                <!-- </div>         -->
              </form>      
            </div>

            <!-- ***** Search End ***** -->
            <!-- ***** Menu Start ***** -->
            <!-- class="active" 이후 선택한 부분에 active 적용-->
            <ul class="nav">
              <li><router-link to="/" >쇼핑</router-link></li> 
              <li><router-link to="/cart/list" >장바구니</router-link></li>
              <li v-if="!member"><router-link to="/member/register" >회원가입</router-link></li>
              <li v-if="!member"><router-link to="/login" >로그인</router-link></li>
              <li v-if="member"><a @click="logout" >로그아웃</a></li>
              <li><router-link to="/mypage/home">프로필 <img src="@/assets/images/profile_images.webp" alt=""/></router-link></li>
            </ul>
            <a class="menu-trigger">
              <span>Menu</span>
            </a>
            <!-- ***** Menu End ***** -->
          </nav>
        </div>
      </div>
    </div>
  </header>
  <!-- ***** Header Area End ***** -->
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';
import { useRouter } from 'vue-router'
  export default { 
    name: "common-header",
    setup () {
      let searchKeyword = ref(null);
      const router = useRouter();
      const search = function () {
        router.push({
          name: 'Home', 
          query: {
            searchKeyword : searchKeyword.value
          }
        })
      }
      let member = ref('');
      const getMember = async () => {
        await axios.get('/member/principal').then((res) => {
        member.value = res.data;});
      }
      (async () => {
        await getMember();
      })();

      axios.defaults.xsrfHeaderName = 'X-CSRF-TOKEN';
      axios.defaults.xsrfCookieName = 'XSRF-TOKEN';

      const logout = () => {
        axios.post('/logout').then(response => {
          alert("로그아웃");
          
        }).catch(error => {
        });
        router.push({
            name: "Home"
          })
      }
      return {
        searchKeyword,
        router,
        search,
        member,
        logout,
      }
    }
  }
</script>

<style>
.logo {
  width: 190px; 
  height: 90px;
}

.logo img {
  width: 100px; 
  height: 100px;
}

.search-input {
  display: flex;
  height: 100%;
}

.search-img {
  width: 35px; 
  height: 35px; 
  position: absolute; 
  top: 28px; 
  left: 9px;
}

.search-img img {
  width: 100%; 
  height: 100%;
}

#searchText {
  margin: 0; 
  margin-top: 20px;
}

.nav {
  display:flex; 
  align-items: center;
}
</style>