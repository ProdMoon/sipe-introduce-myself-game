<script setup>
import { ref, onMounted } from 'vue';
import TextArea from './components/TextArea.vue';
import MusicPlayer from './components/MusicPlayer.vue';

const MY_AGE = 29;
const flowIdx = ref(0);
const scenarioIdx = ref(0);
const content = ref('');
const userName = ref('');
const userAge = ref('');
const userMbti = ref('');
const musicSrc = ref('');

const getAgeComment = () => {
  const age = parseInt(userAge.value);
  if (isNaN(age)) {
    return `${userAge.value}는 나이라고 할 수 없잖아! 숫자가 아닌걸?`;
  }
  if (age === MY_AGE) {
    return `와 ${MY_AGE}살 동갑이구나! 정말 반가운데!`;
  } else if (age < 20) {
    return `${age}살이라니! 미성년자란 말이야? 거짓말을 치고 있군.`;
  } else if (age < MY_AGE) {
    return `${age}살이면.. 나보다 ${MY_AGE - age}살 어리구나. 아직 젊은이잖아? 부럽군!`;
  } else {
    if (age < 40) {
      return `${age}살이면.. 나보다 ${age - MY_AGE}살 많네. 엄청 동안이네? 나랑 비슷한 또래인 줄 알았어.`;
    } else if (age < 60) {
      return `${age}살이면.. 나보다 ${age - MY_AGE}살 많아. 이제부터 늙은이라고 불러도 될까? ... 진정해! 농담이라구.`;
    } else {
      return `세상에, 무려 ${age}살이라니! 거짓말을 치고 있군. 전혀 그 나이로 보이지 않아.`;
    }
  }
};

const getMbtiComment = () => {
  const mbtiTypeComments = {
    ENFP: '너는 ENFP구나! 나와 비슷하게 너도 사람들과 어울리는 것을 좋아하겠는데? 우린 좋은 친구가 될 수 있겠어. 나랑 잘 맞을 것 같아!',
    ENFJ: '너도 나와 같은 ENFJ구나! 우리 같이 사이프의 핵인싸가 되어보자. 비슷한 성격이니 나랑 잘 맞을 것 같아!',
    ENTP: '너는 ENTP구나! 모험적이고 즉흥적인 불꽃같은 사람이겠네! 가끔 번개 약속을 잡으면 같이 놀자! 나랑 잘 맞을 것 같아!',
    ENTJ: '너는 ENTJ구나! 너도 성장과 성취를 중요시 여기니? 목표를 향해 함께 달릴 때 최고의 궁합이겠다. 나랑 잘 맞을 것 같아!',
    ESFP: '너는 ESFP구나! 따뜻하고 사교적인 스타일이겠는데? 내가 좋아하는 성격이야. 나랑 잘 맞을 것 같아!',
    ESFJ: '너는 ESFJ구나! 솔직하고 공감을 잘 해주는 성격이네. 나랑 잘 맞을 것 같아!',
    ESTP: '너는 ESTP구나! 현실 감각이 뛰어난 사업가 스타일! 정말 멋져. 나랑 잘 맞을 것 같아!',
    ESTJ: '너는 ESTJ구나! 질서 정연하고 안정적인 사람이네. 정말 멋져. 나랑 잘 맞을 것 같아!',
    INFP: '너는 INFP구나! 차분하면서도 진실된 성격을 가진 따뜻한 사람이겠네. 나랑 잘 맞을 것 같아!',
    INFJ: '너는 INFJ구나! 직관과 통찰력을 지닌 너와 함께라면 어떤 어려운 일도 풀어나갈 수 있을 것만 같아. 나랑 잘 맞을 것 같아!',
    INTP: '너는 INTP구나! 난 INTP의 논리적이고 객관적인 성격을 정말 좋아해. 나랑 잘 맞을 것 같아!',
    INTJ: '너는 INTJ구나! 지적이고 분석적인 매력, 정말 멋져. 나랑 잘 맞을 것 같아!',
    ISFP: '너는 ISFP구나! 섬세하고 예술적인 사람이라니, 정말 멋져. 나랑 잘 맞을 것 같아!',
    ISFJ: '너는 ISFJ구나! 따뜻하고 책임감 강한 성격을 가지고 있을 것만 같아. 정말 멋져! 나랑 잘 맞을 것 같아!',
    ISTP: '너는 ISTP구나! 현실적이고 임기응변이 강한 스타일이네. 내가 가지고 있지 않은 측면을 보완해줄 수 있을 것만 같아. 나랑 잘 맞을 것 같아!',
    ISTJ: '너는 ISTJ구나! 대충 하는 법 없이 일할 때 확실히 일하고, 놀 때 확실히 노는 사람일 것만 같아. 정말 멋져! 나랑 잘 맞을 것 같아!',
  };

  const mbti = userMbti.value.toUpperCase();
  if (typeof mbtiTypeComments[mbti] !== 'undefined') {
    return mbtiTypeComments[mbti];
  } else {
    return `${mbti}는 MBTI가 아니잖아! MBTI가 뭔지 아직 잘 모르나보네... 하지만 괜찮아! 사이프 OT에서 만나면 내가 뭔지 알려줄게!`;
  }
};

const textReplacer = (text) => {
  text = text.replace('${name}', userName.value);
  text = text.replace('${ageComment}', getAgeComment());
  text = text.replace('${mbtiComment}', getMbtiComment());
  return text;
};

const requestNextScenario = () => {
  const currentFlow = flow[flowIdx.value];
  if (currentFlow.scenario.length - 1 <= scenarioIdx.value) {
    return false;
  }
  scenarioIdx.value += 1;
  content.value = textReplacer(currentFlow.scenario[scenarioIdx.value]);
  return true;
};

const requestNext = async () => {
  const currentFlow = flow[flowIdx.value];
  if (typeof currentFlow.scenario !== 'undefined') {
    if (!requestNextScenario()) {
      flowIdx.value += 1;
      scenarioIdx.value = -1;
      requestNext();
    }
  } else if (typeof currentFlow.exec === 'function') {
    await currentFlow.exec();
    flowIdx.value += 1;
    scenarioIdx.value = -1;
    requestNext();
  }
};

const playIntroMusic = () => {
  musicSrc.value = './intro.mp3';
  setTimeout(() => {
    playVioletCityMusic();
  }, 33000);
};

const playVioletCityMusic = () => {
  musicSrc.value = './violet-city.mp3';
};

const showImage = () => {
  const imageEl = document.getElementById('image-layer');
  imageEl.classList.remove('hidden');
};

const getName = async () => {
  return new Promise((resolve) => {
    userName.value = prompt('너의 이름은 뭐니?');
    resolve();
  });
};

const getAge = async () => {
  return new Promise((resolve) => {
    userAge.value = prompt('나이는 몇 살이니?');
    resolve();
  });
};

const getMbti = async () => {
  return new Promise((resolve) => {
    userMbti.value = prompt('너의 MBTI는 뭐니? (예: ENFJ)');
    resolve();
  });
};

const flow = [
  {
    scenario: ['............'],
  },
  {
    exec: playIntroMusic,
  },
  {
    scenario: [
      '.. 지금이 몇 시지?',
      '.. 이런, 너무 많이 잤군.',
    ],
  },
  {
    exec: showImage,
  },
  {
    scenario: [
      '반가워! 나는 준호라고 해.',
      '너는 누구니?',
    ],
  },
  {
    exec: getName,
  },
  {
    scenario: [
      '${name}.. 좋은 이름이군!',
      '사이프에서 곧 ${name}(이)를 볼 생각에 굉장히 설레.',
      '내 소개를 좀 더 하자면, 지금은 이벤터스라는 회사에서 2년차 유사 풀스택 개발자로 일하고 있어.',
      '주로 백엔드를 하는데 프론트도 하는...',
      '잡부라고 할 수 있지.',
      '아 참, 내 나이는 29살이야.',
      '${name}(이)의 나이도 궁금하네. 몇 살이야?',
    ],
  },
  {
    exec: getAge,
  },
  {
    scenario: [
      '${ageComment}',
      '어쨌든, 이제부터 우리는 친구야! 다 같은 사이퍼들이니까.',
      '${name}(이)가 어떤 성격인지도 궁금하다. 우리 MBTI를 말해볼까?',
      '나는 ENFJ야. 기본적으로 사람을 좋아해! 사람들 앞에 나서는 것도 좋아하지.',
      '그렇다고 매일매일 파티를 즐기는 타입은 아냐... 주기적으로 혼자만의 휴식이 필요하다구!',
      '그럼 ${name}(이)는 어떤 성격일까? 궁금해!',
    ],
  },
  {
    exec: getMbti,
  },
  {
    scenario: [
      '${mbtiComment}',
      '... 말이 좀 길어졌지? 미안해. 나는 사람들과 이야기하는 것을 좋아해서 말이 많아.',
      '... 뭐라고 ${name}? 나랑 이야기하는 것이 재미있다고? 고마워!',
      '그렇다면 좋아. 나에 대해 더 자세히 설명한 글을 준비했어!',
      '사이프 github의 introduce-myself 레포지토리에 올려 두었으니, 한 번 읽어주면 고맙겠어!',
      '그럼, ${name}(이)와 다음에 또 만나자!',
      'OT에 오면 나와 인사해 줘! 그럼, 안녕!',
      '(클릭하면 github로 리디렉트됩니다.)',
    ],
  },
  {
    exec: () => {
      location.href = 'https://github.com/sipe-team/introduce-myself/pull/73';
    },
  },
];

onMounted(() => {
  scenarioIdx.value = -1;
  requestNext();
});
</script>

<template>
  <main class="relative h-screen bg-black">
    <div id="image-layer" class="absolute top-0 flex hidden h-full w-full items-center justify-center">
      <img src="./assets/my-avatar.png" alt="avatar" class="h-1/2" />
    </div>
    <text-area :content="content" :request-next="requestNext" />
    <div class="absolute top-0 right-0 z-10">
      <music-player :src="musicSrc" />
    </div>
  </main>
</template>
