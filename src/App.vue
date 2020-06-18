<template>
  <div id="app" class="antialiased font-nototc relative flex flex-col items-center">
    <header class="header relative w-full px-4 py-2 header--white">
      <div class="container mx-auto flex items-center justify-between">
        <div>
          <img
            class="logo logo--green"
            src="https://api.greenpeace.org.hk/general/logo/GP-logo-2019-TC-green-%5bweb%5d-01.png"
            alt="logo"
          />
          <img
            class="logo logo--white"
            src="https://api.greenpeace.org.hk/general/logo/GP-logo-2019-TC-white-%5bweb%5d-01.png"
            alt="logo"
          />
        </div>
        <div class="bg-cyan nav-button">
          <button v-scroll-to="'#enform'" class="btn">立即聯署</button>
        </div>
      </div>
    </header>
    <!-- end of header -->
    <!-- main -->
    <main class="main relative">
      <div class="grid gap-4">
        <div class="content bg-gray-100">
          <section class="section-hero relative">
            <img
              class="w-full"
              src="https://storage.googleapis.com/planet4-hongkong-stateless/2020/06/934d12df-2020_climate_petition_banner_1200x628-02.jpg"
              alt
            />
            <div class="green-banenr">
              <div class="col-xs-12">
                <h1
                  class="text-green mt-8 mb-4 flex flex-wrap align-center justify-center tracking-wider text-4xl font-bold"
                >
                  <div>爭分奪秒</div>
                  <div class="mx-2"></div>
                  <div>拯救氣候危機</div>
                </h1>
              </div>
            </div>
          </section>
          <Climate />
        </div>
        <slide-x-right-transition :duration="400">
          <aside class="aside-enform enform relative">
            <div id="enform" class="form-container form-container--sticky sticky">
              <div class="form-header mt-2 mb-4 text-2xl text-center font-bold">
                <h2 class="mb-2">爭分奪秒 拯救氣候危機</h2>
                <p class="font-normal text-sm">一同發聲，告訴政府「香港人關注氣候議題」凝聚力量要求正視氣候危機！</p>
              </div>
              <div class="enform-progress my-4">
                <div class="overflow-hidden rounded shadow w-full bg-gray-200">
                  <div
                    class="bg-cyan transition-all font-bold text-white py-1 text-center"
                    v-bind:style="{ width: `${this.signupProgress}%` }"
                    v-show="this.participants && this.goal"
                  >{{ this.participants.toLocaleString() }} 人已聯署</div>
                </div>
              </div>
              <div class="form-body enform"></div>
            </div>
          </aside>
        </slide-x-right-transition>
      </div>
      <div
        class="mobile-button md:hidden tracking-wide shadow overflow-hidden fixed w-full flex items-center justify-center px-4 py-2"
      >
        <div class="scroll-indicator">
          <div class="progress-bar" v-bind:style="{ width: `${this.scrollDepth}%` }"></div>
        </div>
        <button class="form-button" v-scroll-to="'#enform'">{{ mobileBtnText }}</button>
      </div>
    </main>
    <!-- end of main -->
    <!-- footer -->
    <footer class="z-10 footer relative w-full px-4 py-12 bg-gpdarkblue text-white text-sm">
      <div class="container mx-auto">
        <div class="flex flex-wrap">
          <div class="w-full md:w-1/2 mb-8">
            <img
              class="logo mb-8"
              src="https://api.greenpeace.org.hk/general/logo/GP-logo-2019-TC-white-%5bweb%5d-01.png"
              alt="Greenpeace 綠色和平"
            />
            <p
              class="text-sm"
            >綠色和平是獨立的國際環保組織，通過科學研究、政策倡議及和平行動，揭露全球環境問題並提出相應解決方案。我們從不接受任何政府、企業或政治團體的資助，只接受個人的直接捐款，以維持公正獨立。</p>
          </div>
          <div class="w-full md:w-1/2 mb-8">
            <ul
              class="footer-links leading-loose flex flex-col justify-between items-start md:items-end text-right list-none list-inside"
            >
              <a
                target="_blank"
                rel="noopener noreferrer"
                href="http://www.greenpeace.org/hongkong/"
                alt="主頁"
              >主頁</a>
              <a
                target="_blank"
                rel="noopener noreferrer"
                href="https://www.greenpeace.org/hongkong/policies/privacy-and-cookies/"
                alt="私隱政策與個人資料收集聲明"
              >私隱政策與個人資料收集聲明</a>
              <a
                target="_blank"
                rel="noopener noreferrer"
                href="http://www.greenpeace.org/hk/about/contact/"
                alt="聯絡我們"
              >聯絡我們</a>
              <a
                target="_blank"
                rel="noopener noreferrer"
                href="https://www.greenpeace.org/hongkong/about/overview/"
                alt="關於綠色和平"
              >關於綠色和平</a>
            </ul>
          </div>
          <div class="w-full mb-8">
            <div class="border-t border-gray-400 px-4"></div>
            <div class="text-sm mt-4">© GREENPEACE 2020</div>
          </div>
        </div>
      </div>
    </footer>
    <!-- end of footer -->
  </div>
</template>

<script>
import NProgress from "nprogress";
NProgress.configure({
  showSpinner: false
});
import { mainShare, whatsAppShare } from "@/share.js";
import { supermarkets, supermarketLogos } from "@/supermarkets.js";
import Climate from "./components/Climate.vue";
//
const createBirthYearList = function() {
  const birthYear = document.getElementById("en__field_supporter_NOT_TAGGED_6");
  if (birthYear) {
    let c = document.createDocumentFragment();
    let cLabel = document.createElement("option");
    cLabel.value = "";
    cLabel.innerHTML = "出生年份";
    c.append(cLabel);
    let thisYear = new Date().getFullYear();
    for (let i = thisYear; i >= thisYear - 99; i--) {
      let opt = document.createElement("option");
      opt.value = "01/01/" + i.toString();
      opt.innerHTML = i.toString();
      c.appendChild(opt);
    }
    birthYear.innerHTML = "";
    birthYear.append(c);
  }
};
const enFormType = function() {
  const enform = document.querySelector("form.en__component");
  const email = document.getElementById("en__field_supporter_emailAddress");
  const phone = document.getElementById("en__field_supporter_phoneNumber");
  const formFields = document.querySelectorAll(".en__field--text");
  //
  if (email && phone) {
    email.setAttribute("type", "email");
    phone.setAttribute("type", "tel");
  }
  if (formFields) {
    formFields.forEach(field => {
      let textField = field.querySelector(".en__field__input--text");
      let label = field.querySelector(".en__field__label").textContent;
      textField.placeholder = label;
    });
  }
  if (enform) {
    enform.setAttribute("autocomplete", "off");
  }
};
const decorForm = function() {
  const enFields = document.querySelectorAll(".en__field");
  const formFields = document.querySelectorAll(".en__field");
  const formLabels = document.querySelectorAll(".en__field label");
  const formInputs = document.querySelectorAll(
    ".en__field__input:not(#en__field_supporter_questions_7275), #en__field_supporter_NOT_TAGGED_6"
  );
  const enSubmit = document.querySelector(".en__submit button");
  if (formFields) {
    Array.from(formFields).map(field => {
      field.classList.add("form-row");
    });
  }
  if (formLabels) {
    Array.from(formLabels).map(label => {
      label.classList.add("form-label");
    });
  }
  if (formInputs) {
    Array.from(formInputs).map(input => {
      input.classList.add("form-input");
    });
  }
  if (enSubmit) {
    enSubmit.classList.add("form-button");
  }
};
const appendForm = function() {
  const nativeForm = document.querySelector("form.en__component--page");
  const enFormWrapper = document.querySelector(".form-body");
  if (nativeForm && enFormWrapper) {
    enFormWrapper.appendChild(nativeForm);
  }
};
//
export default {
  name: "App",
  components: { Climate },
  data() {
    return {
      supermarkets: supermarkets,
      supermarketLogos: supermarketLogos,
      isMobile: false,
      scrollDepth: 0,
      currentPage: 0,
      showMobileButton: true,
      showFormModal: true,
      formSumitted: false,
      participants: 0,
      goal: 0
    };
  },
  methods: {
    scrollIntoView(evt) {
      evt.preventDefault();
      const href = evt.target.getAttribute("href");
      const el = href ? document.querySelector(href) : null;
      if (el) {
        this.$refs.content.scrollTop = el.offsetTop;
      }
    },
    checkMobile() {
      var isMobile = window.matchMedia("screen and (max-width:767px)").matches;
      this.isMobile = isMobile;
      return isMobile;
    },
    getDocumentHeight() {
      return Math.max(
        document.body.scrollHeight,
        document.body.offsetHeight,
        document.documentElement.clientHeight,
        document.documentElement.scrollHeight,
        document.documentElement.offsetHeight
      );
    },
    getWindowHeight() {
      return (
        window.innerHeight ||
        document.documentElement.clientHeight ||
        document.getElementsByTagName("body")[0].clientHeight
      );
    },
    getScrollTop() {
      return window.pageYOffset !== undefined
        ? window.pageYOffset
        : (
            document.documentElement ||
            document.body.parentNode ||
            document.body
          ).scrollTop;
    },
    handleScroll() {
      let scroll = this.getScrollTop() / this.innerHeight;
      this.scrollDepth = Math.round(scroll * 100);
    }
  },
  computed: {
    mobileBtnText() {
      return this.formSubmitted ? "感謝您聯署發聲" : "立即聯署";
    },
    innerHeight() {
      return this.getDocumentHeight() - this.getWindowHeight();
    },
    signupProgress() {
      return this.participants
        ? Math.round((this.participants / this.goal) * 100)
        : 0;
    }
  },
  created() {
    NProgress.start();
    window.addEventListener("scroll", this.handleScroll);
    //
    const page = window.pageJson.pageNumber;
    this.currentPage = page;
    if (page == 2) {
      this.formSubmitted = true;
      const shareBtn = document.querySelector(".button--share");
      const whatsappBtn = document.querySelector(".button--whatsappshare");
      if (shareBtn) {
        shareBtn.addEventListener("click", mainShare);
      }
      if (whatsappBtn) {
        whatsappBtn.addEventListener("click", whatsAppShare);
      }
    }
  },
  mounted() {
    this.checkMobile();
    enFormType();
    createBirthYearList();
    appendForm();
    decorForm();
    //
    const widgetEndPoint =
      "https://cors-anywhere.small-service.gpeastasia.org/https://act.greenpeace.org/page/widget/434094";
    fetch(widgetEndPoint)
      .then(resp => resp.json())
      .then(data => {
        let campaignList = data.data.rows;
        let totalRegistration = 0;
        campaignList.forEach(campaign => {
          totalRegistration =
            totalRegistration + parseInt(campaign.columns[4].value);
        });
        this.participants = totalRegistration;
        return data.jsonContent;
      })
      .then(jsonContent => {
        this.goal = JSON.parse(jsonContent).goal;
        // console.log(this.participants);
        // console.log(this.goal);
        // console.log(this.signupProgress);
      })
      .catch(error => {
        this.participants = 0;
      });
    this.$nextTick(() => {
      NProgress.done();
    });
  },
  destroyed() {
    document.removeEventListener("scroll", this.handleScroll);
    alert("Please refresh the page");
  }
};
</script>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
}
</style>
