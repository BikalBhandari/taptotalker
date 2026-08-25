<script setup>
import { computed, onMounted, ref, watch } from 'vue';
import {
  ArrowLeft,
  Apple,
  Bed,
  BookOpen,
  Brain,
  Bus,
  Check,
  CircleAlert,
  Coffee,
  Cookie,
  Drumstick,
  Ear,
  Eye,
  Gamepad2,
  GlassWater,
  Heart,
  Home,
  IceCreamBowl,
  Laugh,
  MapPin,
  MessageCircle,
  Maximize2,
  Moon,
  Music,
  Palette,
  PersonStanding,
  Pizza,
  Sandwich,
  Settings,
  RotateCcw,
  Smile,
  Sparkles,
  Sun,
  ThumbsDown,
  ThumbsUp,
  Toilet,
  Utensils,
  Wind,
  X,
} from '@lucide/vue';

const flow = {
  prompt: 'Choose a message starter',
  options: [
    {
      id: 'want',
      label: 'I want to',
      icon: Heart,
      tone: 'rose',
      prompt: 'What do you want?',
      options: [
        {
          id: 'eat',
          label: 'Eat',
          icon: Utensils,
          tone: 'amber',
          prompt: 'What would you like to eat?',
          options: [
            { id: 'cookies', label: 'Cookies', icon: Cookie, tone: 'amber' },
            {
              id: 'chicken',
              label: 'Chicken',
              icon: Drumstick,
              tone: 'orange',
              options: [
                { id: 'nuggets', label: 'Nuggets', icon: Drumstick, tone: 'orange', minMode: 'advanced' },
                { id: 'strips', label: 'Strips', icon: Drumstick, tone: 'amber', minMode: 'advanced' },
                { id: 'warm', label: 'Warm', icon: Coffee, tone: 'rose', minMode: 'advanced' },
                { id: 'cold', label: 'Cold', icon: GlassWater, tone: 'sky', minMode: 'advanced' },
              ],
            },
            { id: 'sandwich', label: 'Sandwich', icon: Sandwich, tone: 'yellow' },
            { id: 'pizza', label: 'Pizza', icon: Pizza, tone: 'rose' },
            { id: 'apple', label: 'Apple', icon: Apple, tone: 'emerald' },
            {
              id: 'ice-cream',
              label: 'Ice cream',
              icon: IceCreamBowl,
              tone: 'violet',
              options: [
                { id: 'vanilla', label: 'Vanilla', icon: IceCreamBowl, tone: 'yellow', minMode: 'advanced' },
                { id: 'chocolate', label: 'Chocolate', icon: IceCreamBowl, tone: 'orange', minMode: 'advanced' },
                { id: 'strawberry', label: 'Strawberry', icon: IceCreamBowl, tone: 'rose', minMode: 'advanced' },
              ],
            },
          ],
        },
        {
          id: 'drink',
          label: 'Drink',
          icon: GlassWater,
          tone: 'sky',
          prompt: 'What would you like to drink?',
          options: [
            {
              id: 'water',
              label: 'Water',
              icon: GlassWater,
              tone: 'sky',
              options: [
                { id: 'small', label: 'Small', icon: GlassWater, tone: 'sky', minMode: 'advanced' },
                { id: 'big', label: 'Big', icon: GlassWater, tone: 'cyan', minMode: 'advanced' },
                { id: 'ice', label: 'Ice', icon: Sparkles, tone: 'teal', minMode: 'advanced' },
                { id: 'no-ice', label: 'No ice', icon: X, tone: 'violet', minMode: 'advanced' },
              ],
            },
            { id: 'juice', label: 'Juice', icon: Apple, tone: 'orange' },
            { id: 'milk', label: 'Milk', icon: Coffee, tone: 'teal' },
            { id: 'warm-drink', label: 'Warm drink', icon: Coffee, tone: 'amber' },
          ],
        },
        {
          id: 'rest',
          label: 'Rest',
          icon: Bed,
          tone: 'indigo',
          prompt: 'Where do you want to rest?',
          options: [
            {
              id: 'bed',
              label: 'Bed',
              icon: Bed,
              tone: 'indigo',
              options: [
                { id: 'blanket', label: 'Blanket', icon: Bed, tone: 'violet', minMode: 'advanced' },
                { id: 'pillow', label: 'Pillow', icon: Bed, tone: 'sky', minMode: 'advanced' },
                { id: 'lights-off', label: 'Lights off', icon: Moon, tone: 'indigo', minMode: 'advanced' },
              ],
            },
            { id: 'chair', label: 'Chair', icon: PersonStanding, tone: 'teal' },
            { id: 'home', label: 'Home', icon: Home, tone: 'emerald' },
            { id: 'quiet', label: 'Quiet room', icon: Sparkles, tone: 'violet' },
          ],
        },
        {
          id: 'play',
          label: 'Play',
          icon: Gamepad2,
          tone: 'emerald',
          prompt: 'What do you want to play?',
          options: [
            { id: 'game', label: 'Game', icon: Gamepad2, tone: 'emerald' },
            { id: 'music', label: 'Music', icon: Music, tone: 'violet' },
            { id: 'drawing', label: 'Drawing', icon: Palette, tone: 'rose' },
            { id: 'read', label: 'Read', icon: BookOpen, tone: 'cyan' },
          ],
        },
        {
          id: 'go',
          label: 'Go to',
          icon: MapPin,
          tone: 'teal',
          prompt: 'Where do you want to go?',
          options: [
            { id: 'bathroom', label: 'Bathroom', icon: Toilet, tone: 'teal' },
            { id: 'outside', label: 'Outside', icon: Sun, tone: 'yellow' },
            { id: 'car', label: 'Car', icon: Bus, tone: 'sky' },
            { id: 'home', label: 'Home', icon: Home, tone: 'emerald' },
          ],
        },
        {
          id: 'help',
          label: 'Get help',
          icon: MessageCircle,
          tone: 'cyan',
          prompt: 'What help do you need?',
          options: [
            { id: 'pain', label: 'Pain', icon: CircleAlert, tone: 'rose' },
            { id: 'bathroom', label: 'Bathroom', icon: Toilet, tone: 'teal' },
            {
              id: 'talk',
              label: 'Talk',
              icon: Smile,
              tone: 'yellow',
              options: [
                { id: 'mom', label: 'Mom', icon: Heart, tone: 'rose', minMode: 'advanced' },
                { id: 'dad', label: 'Dad', icon: Heart, tone: 'sky', minMode: 'advanced' },
                { id: 'teacher', label: 'Teacher', icon: BookOpen, tone: 'cyan', minMode: 'advanced' },
                { id: 'caregiver', label: 'Caregiver', icon: PersonStanding, tone: 'emerald', minMode: 'advanced' },
              ],
            },
            { id: 'stuck', label: 'Stuck', icon: X, tone: 'orange' },
          ],
        },
      ],
    },
    {
      id: 'feel',
      label: 'I feel',
      icon: Smile,
      tone: 'yellow',
      prompt: 'How do you feel?',
      options: [
        { id: 'happy', label: 'Happy', icon: Smile, tone: 'yellow' },
        { id: 'excited', label: 'Excited', icon: Laugh, tone: 'amber' },
        { id: 'tired', label: 'Tired', icon: Moon, tone: 'indigo' },
        { id: 'sick', label: 'Sick', icon: Heart, tone: 'rose' },
        { id: 'sad', label: 'Sad', icon: Heart, tone: 'sky' },
        { id: 'scared', label: 'Scared', icon: CircleAlert, tone: 'orange' },
        { id: 'angry', label: 'Angry', icon: X, tone: 'rose' },
        { id: 'confused', label: 'Confused', icon: Brain, tone: 'violet' },
      ],
    },
    {
      id: 'need',
      label: 'I need',
      icon: MessageCircle,
      tone: 'cyan',
      prompt: 'What do you need?',
      options: [
        { id: 'water', label: 'Water', icon: GlassWater, tone: 'sky' },
        { id: 'food', label: 'Food', icon: Utensils, tone: 'amber' },
        { id: 'help', label: 'Help', icon: Heart, tone: 'rose' },
        { id: 'bathroom', label: 'Bathroom', icon: Toilet, tone: 'teal' },
        { id: 'break', label: 'Break', icon: Moon, tone: 'indigo' },
        { id: 'medicine', label: 'Medicine', icon: CircleAlert, tone: 'rose' },
        { id: 'blanket', label: 'Blanket', icon: Bed, tone: 'violet' },
        { id: 'space', label: 'Space', icon: Wind, tone: 'cyan' },
      ],
    },
    {
      id: 'person',
      label: 'I want',
      icon: PersonStanding,
      tone: 'emerald',
      prompt: 'Who do you want?',
      options: [
        { id: 'mom', label: 'Mom', icon: Heart, tone: 'rose' },
        { id: 'dad', label: 'Dad', icon: Heart, tone: 'sky' },
        { id: 'friend', label: 'Friend', icon: Smile, tone: 'yellow' },
        { id: 'teacher', label: 'Teacher', icon: BookOpen, tone: 'cyan' },
        { id: 'doctor', label: 'Doctor', icon: CircleAlert, tone: 'teal' },
        { id: 'caregiver', label: 'Caregiver', icon: PersonStanding, tone: 'emerald' },
      ],
    },
    {
      id: 'answer',
      label: 'Answer',
      icon: MessageCircle,
      tone: 'violet',
      prompt: 'What is your answer?',
      options: [
        { id: 'yes', label: 'Yes', icon: ThumbsUp, tone: 'emerald' },
        { id: 'no', label: 'No', icon: ThumbsDown, tone: 'rose' },
        { id: 'maybe', label: 'Maybe', icon: MessageCircle, tone: 'violet' },
        { id: 'more', label: 'More', icon: Sparkles, tone: 'cyan' },
        { id: 'finished', label: 'Finished', icon: Check, tone: 'teal' },
        { id: 'stop', label: 'Stop', icon: X, tone: 'orange' },
      ],
    },
    {
      id: 'not-okay',
      label: 'Not okay',
      icon: CircleAlert,
      tone: 'rose',
      prompt: 'What feels not okay?',
      options: [
        { id: 'sick', label: 'Sick', icon: Heart, tone: 'rose' },
        { id: 'scared', label: 'Scared', icon: CircleAlert, tone: 'orange' },
        { id: 'loud', label: 'Too loud', icon: Ear, tone: 'orange' },
        { id: 'tired', label: 'Tired', icon: Moon, tone: 'indigo' },
        { id: 'body', label: 'Body', icon: PersonStanding, tone: 'emerald' },
        { id: 'help', label: 'Need help', icon: Heart, tone: 'cyan' },
      ],
    },
    {
      id: 'see',
      label: 'I see',
      icon: Eye,
      tone: 'sky',
      prompt: 'What do you see?',
      options: [
        { id: 'person', label: 'Person', icon: PersonStanding, tone: 'emerald' },
        { id: 'animal', label: 'Animal', icon: Eye, tone: 'yellow' },
        { id: 'car', label: 'Car', icon: Bus, tone: 'sky' },
        { id: 'food', label: 'Food', icon: Utensils, tone: 'amber' },
        { id: 'outside', label: 'Outside', icon: Sun, tone: 'yellow' },
        { id: 'something-scary', label: 'Scary thing', icon: CircleAlert, tone: 'rose' },
      ],
    },
    {
      id: 'hear',
      label: 'I hear',
      icon: Ear,
      tone: 'teal',
      prompt: 'What do you hear?',
      options: [
        { id: 'music', label: 'Music', icon: Music, tone: 'violet' },
        { id: 'loud', label: 'Loud sound', icon: Ear, tone: 'orange' },
        { id: 'voice', label: 'Voice', icon: MessageCircle, tone: 'cyan' },
        { id: 'quiet', label: 'Quiet', icon: Sparkles, tone: 'sky' },
      ],
    },
  ],
};

const selectedPath = ref([]);
const vocabularyMode = ref('guided');
const pendingVocabularyMode = ref(vocabularyMode.value);
const boardMode = ref('default');
const pendingBoardMode = ref(boardMode.value);
const showCaregiverControls = ref(false);
const settingsMessage = ref('');
const cardCustomizations = ref({});
const selectedEditGroupId = ref('home');

const customizationStorageKey = 'taptotalker-card-customizations';
const boardModes = [
  { id: 'default', label: 'Default' },
  { id: 'custom', label: 'Custom' },
  { id: 'edit', label: 'Edit' },
];

const vocabularyModes = [
  { id: 'simple', label: 'Simple', limit: 5, rank: 0, maxSteps: 3 },
  { id: 'intermediate', label: 'Intermediate', limit: null, rank: 1, maxSteps: 3 },
  { id: 'guided', label: 'Guided', limit: 6, rank: 1, maxSteps: 3 },
  { id: 'advanced', label: 'Advanced', limit: null, rank: 2, maxSteps: 4 },
];

const visualCues = {
  animal: '🐶',
  angry: '😠',
  answer: '✅',
  apple: '🍎',
  bathroom: '🚽',
  bed: '🛏️',
  blanket: '🧣',
  body: '🧍',
  break: '⏸️',
  car: '🚗',
  caregiver: '🤝',
  chair: '🪑',
  chicken: '🍗',
  confused: '❓',
  cookies: '🍪',
  dad: '👨',
  doctor: '🩺',
  drawing: '🎨',
  drink: '🥤',
  ear: '👂',
  eat: '🍽️',
  excited: '🤩',
  eye: '👁️',
  feel: '🙂',
  finished: '🏁',
  food: '🍽️',
  friend: '🧑‍🤝‍🧑',
  game: '🎮',
  go: '📍',
  happy: '😊',
  head: '🧠',
  hear: '👂',
  help: '🆘',
  home: '🏠',
  'not-okay': '🙁',
  'ice-cream': '🍨',
  ice: '🧊',
  juice: '🧃',
  big: '➕',
  chocolate: '🍫',
  cold: '🧊',
  'lights-off': '🌙',
  loud: '🔊',
  maybe: '🤔',
  medicine: '💊',
  milk: '🥛',
  mom: '👩',
  more: '➕',
  mouth: '👄',
  music: '🎵',
  need: '💬',
  no: '👎',
  outside: '☀️',
  pain: '🤕',
  person: '🧍',
  pizza: '🍕',
  play: '🎮',
  quiet: '🤫',
  read: '📖',
  rest: '🛏️',
  sad: '😢',
  sandwich: '🥪',
  scared: '😟',
  see: '👁️',
  sick: '🤒',
  'something-scary': '⚠️',
  space: '🌬️',
  stomach: '🤢',
  stop: '🛑',
  strawberry: '🍓',
  stuck: '🚫',
  strips: '🍗',
  talk: '💬',
  teacher: '👩‍🏫',
  tired: '😴',
  voice: '🗣️',
  want: '❤️',
  'warm-drink': '☕',
  warm: '♨️',
  water: '💧',
  yes: '👍',
};

const currentNode = computed(() =>
  selectedPath.value.reduce((node, index) => node.options[index], flow),
);

const currentOptions = computed(() => currentNode.value.options ?? []);
const activeVocabularyMode = computed(() => vocabularyModes.find((mode) => mode.id === vocabularyMode.value) ?? vocabularyModes[1]);
const pendingActiveVocabularyMode = computed(() => vocabularyModes.find((mode) => mode.id === pendingVocabularyMode.value) ?? vocabularyModes[1]);
const visibleOptions = computed(() => {
  return optionsForMode(currentOptions.value, activeVocabularyMode.value);
});
const editGroups = computed(() => {
  const groups = [
    {
      id: 'home',
      title: 'Home screen',
      options: optionsForMode(flow.options, pendingActiveVocabularyMode.value),
    },
  ];

  function collectScreens(options, parentLabel = '') {
    optionsForMode(options, pendingActiveVocabularyMode.value).forEach((option) => {
      if (!option.options) return;

      const title = parentLabel ? `${parentLabel} / ${displayLabelFor(option)}` : displayLabelFor(option);
      groups.push({
        id: option.id,
        title,
        options: optionsForMode(option.options, pendingActiveVocabularyMode.value),
      });

      collectScreens(option.options, title);
    });
  }

  collectScreens(flow.options);
  return groups;
});
const activeEditGroup = computed(() =>
  editGroups.value.find((group) => group.id === selectedEditGroupId.value) ?? editGroups.value[0],
);
const phrase = computed(() => selectedPath.value.map((_, index) => displayLabelFor(nodeAt(index))).join(' '));
const maxSteps = computed(() => activeVocabularyMode.value.maxSteps);
const flowStepCount = computed(() => Math.min(maxSteps.value, selectedPath.value.length + remainingStepCount(currentNode.value)));
const currentStepNumber = computed(() => Math.min(selectedPath.value.length + 1, flowStepCount.value));
const stepLabel = computed(() => `Step ${currentStepNumber.value} of ${flowStepCount.value}`);
const isComplete = computed(() => selectedPath.value.length === flowStepCount.value || !currentOptions.value.length);
const canUseFullscreen = computed(() => typeof document !== 'undefined' && Boolean(document.documentElement.requestFullscreen));
const activeBoardMode = computed(() => boardModes.find((mode) => mode.id === boardMode.value) ?? boardModes[0]);
const canUseCustomCards = computed(() => boardMode.value === 'custom' || boardMode.value === 'edit');

onMounted(() => {
  const savedCustomizations = window.localStorage.getItem(customizationStorageKey);
  if (!savedCustomizations) return;
  try {
    cardCustomizations.value = JSON.parse(savedCustomizations);
  } catch {
    window.localStorage.removeItem(customizationStorageKey);
  }
});

watch(editGroups, (groups) => {
  if (!groups.some((group) => group.id === selectedEditGroupId.value)) {
    selectedEditGroupId.value = groups[0]?.id ?? 'home';
  }
});

function nodeAt(pathIndex) {
  return selectedPath.value.slice(0, pathIndex + 1).reduce((node, index) => node.options[index], flow);
}

function chooseOption(index) {
  if (selectedPath.value.length >= maxSteps.value) return;
  const selectedOption = visibleOptions.value[index];
  selectedPath.value = [...selectedPath.value, index];
  speakText(displayLabelFor(selectedOption));
}

function goBack() {
  selectedPath.value = selectedPath.value.slice(0, -1);
}

function resetFlow() {
  selectedPath.value = [];
}

function optionsForMode(options, mode) {
  const availableOptions = options.filter((option) => {
    if (!option.minMode) return true;
    const minMode = vocabularyModes.find((mode) => mode.id === option.minMode);
    return mode.rank >= (minMode?.rank ?? 0);
  });

  if (!mode.limit) return availableOptions;
  return availableOptions.slice(0, mode.limit);
}

function remainingStepCount(node) {
  const options = optionsForMode(node.options ?? [], activeVocabularyMode.value);
  if (!options.length) return 0;

  const childStepCounts = options.map((option) => 1 + remainingStepCount(option));
  return Math.max(...childStepCounts);
}

function saveSettings() {
  vocabularyMode.value = pendingVocabularyMode.value;
  boardMode.value = pendingBoardMode.value;
  settingsMessage.value = `${activeVocabularyMode.value.label} vocabulary and ${activeBoardMode.value.label.toLowerCase()} cards saved.`;
  if (boardMode.value === 'edit') return;
  window.setTimeout(() => {
    showCaregiverControls.value = false;
    settingsMessage.value = '';
  }, 900);
}

function saveCustomCards() {
  window.localStorage.setItem(customizationStorageKey, JSON.stringify(cardCustomizations.value));
  boardMode.value = 'custom';
  pendingBoardMode.value = 'custom';
  settingsMessage.value = 'Custom cards saved.';
  window.setTimeout(() => {
    showCaregiverControls.value = false;
    settingsMessage.value = '';
  }, 900);
}

function enterFullscreen() {
  if (!document.documentElement.requestFullscreen) return;
  document.documentElement.requestFullscreen();
}

function speakPhrase() {
  speakText(phrase.value);
}

function speakText(text) {
  if (!text || !('speechSynthesis' in window)) return;
  window.speechSynthesis.cancel();
  window.speechSynthesis.speak(new SpeechSynthesisUtterance(text));
}

function visualCueFor(option) {
  if (canUseCustomCards.value && cardCustomizations.value[option.id]?.image) return '';
  if (canUseCustomCards.value && cardCustomizations.value[option.id]?.emoji) {
    return cardCustomizations.value[option.id].emoji;
  }
  return option.emoji ?? visualCues[option.id] ?? '🔹';
}

function displayLabelFor(option) {
  if (!canUseCustomCards.value) return option.label;
  return cardCustomizations.value[option.id]?.label || option.label;
}

function displayImageFor(option) {
  if (!canUseCustomCards.value) return '';
  return cardCustomizations.value[option.id]?.image || '';
}

function updateCardLabel(option, label) {
  cardCustomizations.value = {
    ...cardCustomizations.value,
    [option.id]: {
      ...cardCustomizations.value[option.id],
      label,
    },
  };
}

function updateCardImage(option, event) {
  const file = event.target.files?.[0];
  if (!file) return;

  const reader = new FileReader();
  reader.onload = () => {
    cardCustomizations.value = {
      ...cardCustomizations.value,
      [option.id]: {
        ...cardCustomizations.value[option.id],
        image: reader.result,
      },
    };
  };
  reader.readAsDataURL(file);
}

function resetCardImage(option) {
  const existing = cardCustomizations.value[option.id];
  if (!existing) return;

  const { image, ...nextCustomization } = existing;
  cardCustomizations.value = {
    ...cardCustomizations.value,
    [option.id]: nextCustomization,
  };
}

const toneClasses = {
  amber: 'border-amber-200 bg-amber-50 text-amber-950 shadow-amber-100/70',
  cyan: 'border-cyan-200 bg-cyan-50 text-cyan-950 shadow-cyan-100/70',
  emerald: 'border-emerald-200 bg-emerald-50 text-emerald-950 shadow-emerald-100/70',
  indigo: 'border-indigo-200 bg-indigo-50 text-indigo-950 shadow-indigo-100/70',
  orange: 'border-orange-200 bg-orange-50 text-orange-950 shadow-orange-100/70',
  rose: 'border-rose-200 bg-rose-50 text-rose-950 shadow-rose-100/70',
  sky: 'border-sky-200 bg-sky-50 text-sky-950 shadow-sky-100/70',
  teal: 'border-teal-200 bg-teal-50 text-teal-950 shadow-teal-100/70',
  violet: 'border-violet-200 bg-violet-50 text-violet-950 shadow-violet-100/70',
  yellow: 'border-yellow-200 bg-yellow-50 text-yellow-950 shadow-yellow-100/70',
};
</script>

<template>
  <main class="min-h-screen bg-[#f5f7fb]">
    <section class="safe-shell mx-auto flex min-h-screen w-full max-w-7xl select-none flex-col sm:px-6 lg:px-8">
      <header class="flex flex-col gap-4 border-b border-slate-200/80 pb-4 sm:flex-row sm:items-center sm:justify-between">
        <div>
          <p class="text-sm font-semibold uppercase tracking-[0.18em] text-slate-500">TapToTalker</p>
          <h1 class="mt-1 text-3xl font-bold text-slate-950 sm:text-4xl">Communication board</h1>
        </div>
        <div class="flex items-center gap-2">
          <button
            v-if="canUseFullscreen"
            type="button"
            class="inline-flex h-12 w-12 items-center justify-center rounded-full border border-slate-200 bg-white text-slate-700 shadow-sm transition hover:border-slate-300 hover:text-slate-950 focus:outline-none focus:ring-4 focus:ring-cyan-200"
            aria-label="Fullscreen"
            title="Fullscreen"
            @click="enterFullscreen"
          >
            <Maximize2 class="h-5 w-5" />
          </button>
          <button
            type="button"
            class="inline-flex h-12 w-12 items-center justify-center rounded-full border border-slate-200 bg-white text-slate-700 shadow-sm transition hover:border-slate-300 hover:text-slate-950 focus:outline-none focus:ring-4 focus:ring-cyan-200 disabled:cursor-not-allowed disabled:opacity-40"
            :disabled="!selectedPath.length"
            aria-label="Go back"
            title="Go back"
            @click="goBack"
          >
            <ArrowLeft class="h-5 w-5" />
          </button>
          <button
            type="button"
            class="inline-flex h-12 w-12 items-center justify-center rounded-full border border-slate-200 bg-white text-slate-700 shadow-sm transition hover:border-slate-300 hover:text-slate-950 focus:outline-none focus:ring-4 focus:ring-cyan-200"
            aria-label="Start over"
            title="Start over"
            @click="resetFlow"
          >
            <RotateCcw class="h-5 w-5" />
          </button>
          <button
            type="button"
            class="inline-flex h-12 w-12 items-center justify-center rounded-full border border-slate-200 bg-white text-slate-700 shadow-sm transition hover:border-slate-300 hover:text-slate-950 focus:outline-none focus:ring-4 focus:ring-cyan-200"
            aria-label="Caregiver settings"
            title="Caregiver settings"
            @click="showCaregiverControls = !showCaregiverControls"
          >
            <Settings class="h-5 w-5" />
          </button>
        </div>
      </header>

      <div class="flex flex-1 flex-col gap-4 py-4">
        <div class="rounded-lg border border-slate-200 bg-white p-3 shadow-soft">
          <div class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
            <div class="flex items-center gap-3">
              <span class="rounded-full bg-slate-100 px-3 py-1 text-sm font-semibold text-slate-600">{{ stepLabel }}</span>
              <Check v-if="isComplete" class="h-6 w-6 text-emerald-600" />
            </div>
            <div class="grid w-full grid-cols-3 gap-2 sm:w-72">
              <div
                v-for="step in flowStepCount"
                :key="step"
                class="h-2 rounded-full"
                :class="step <= selectedPath.length ? 'bg-cyan-500' : 'bg-slate-200'"
              />
            </div>
          </div>

          <div v-if="selectedPath.length" class="mt-3 flex flex-wrap gap-2">
            <span
              v-for="(_, index) in selectedPath"
              :key="index"
              class="rounded-full border border-slate-200 bg-white px-4 py-2 text-sm font-semibold text-slate-700"
            >
              {{ displayLabelFor(nodeAt(index)) }}
            </span>
          </div>
        </div>

        <div v-if="showCaregiverControls" class="rounded-lg border border-slate-200 bg-white p-4 shadow-soft">
          <p class="text-sm font-semibold text-slate-500">Caregiver settings</p>
          <div class="mt-3 grid gap-3 lg:grid-cols-[1fr_1fr_auto] lg:items-end">
            <label class="block">
              <span class="text-sm font-bold text-slate-800">Vocabulary mode</span>
              <select
                v-model="pendingVocabularyMode"
                class="mt-2 min-h-12 w-full rounded-lg border border-slate-200 bg-white px-4 text-base font-semibold text-slate-900 shadow-sm focus:outline-none focus:ring-4 focus:ring-cyan-200"
              >
                <option
                  v-for="mode in vocabularyModes"
                  :key="mode.id"
                  :value="mode.id"
                >
                  {{ mode.label }}
                </option>
              </select>
            </label>
            <label class="block">
              <span class="text-sm font-bold text-slate-800">Card mode</span>
              <select
                v-model="pendingBoardMode"
                class="mt-2 min-h-12 w-full rounded-lg border border-slate-200 bg-white px-4 text-base font-semibold text-slate-900 shadow-sm focus:outline-none focus:ring-4 focus:ring-cyan-200"
              >
                <option
                  v-for="mode in boardModes"
                  :key="mode.id"
                  :value="mode.id"
                >
                  {{ mode.label }}
                </option>
              </select>
            </label>
            <button
              type="button"
              class="inline-flex min-h-12 items-center justify-center rounded-lg bg-slate-950 px-5 text-base font-bold text-white shadow-lg shadow-slate-300 transition hover:bg-slate-800 focus:outline-none focus:ring-4 focus:ring-cyan-200"
              @click="saveSettings"
            >
              Save settings
            </button>
          </div>
          <p class="mt-3 text-sm leading-relaxed text-slate-500">
            Current modes: {{ activeVocabularyMode.label }} vocabulary, {{ activeBoardMode.label }} cards. Edit mode lets caregivers rename cards and upload custom images.
          </p>
          <div
            v-if="pendingBoardMode === 'edit' || boardMode === 'edit'"
            class="mt-4 border-t border-slate-100 pt-4"
          >
            <div class="flex flex-col gap-3 sm:flex-row sm:items-center sm:justify-between">
              <div>
                <p class="text-sm font-semibold text-slate-500">Edit cards</p>
                <p class="mt-1 text-sm text-slate-500">Showing cards available in {{ pendingActiveVocabularyMode.label }} mode.</p>
              </div>
              <button
                type="button"
                class="inline-flex min-h-11 items-center justify-center rounded-lg bg-emerald-700 px-5 text-sm font-bold text-white shadow-sm transition hover:bg-emerald-800 focus:outline-none focus:ring-4 focus:ring-emerald-200"
                @click="saveCustomCards"
              >
                Save as custom
              </button>
            </div>
            <div class="mt-4 flex gap-2 overflow-x-auto rounded-lg bg-slate-50 p-2">
              <button
                v-for="group in editGroups"
                :key="group.id"
                type="button"
                class="min-h-10 shrink-0 rounded-md px-4 text-sm font-bold transition focus:outline-none focus:ring-4 focus:ring-cyan-200"
                :class="activeEditGroup.id === group.id ? 'bg-slate-950 text-white shadow-sm' : 'bg-white text-slate-700 hover:bg-slate-100'"
                @click="selectedEditGroupId = group.id"
              >
                {{ group.title }}
              </button>
            </div>
            <div class="mt-4 grid gap-3 md:grid-cols-2 xl:grid-cols-3">
              <div
                v-for="option in activeEditGroup.options"
                :key="option.id"
                class="rounded-lg border border-slate-200 bg-slate-50 p-3"
              >
                <div class="flex items-center gap-3">
                  <div class="flex h-16 w-16 shrink-0 items-center justify-center overflow-hidden rounded-lg border border-slate-200 bg-white text-4xl">
                    <img
                      v-if="displayImageFor(option)"
                      :src="displayImageFor(option)"
                      :alt="`${displayLabelFor(option)} custom image`"
                      class="h-full w-full object-cover"
                    />
                    <span v-else aria-hidden="true">{{ visualCueFor(option) }}</span>
                  </div>
                  <label class="min-w-0 flex-1">
                    <span class="text-xs font-bold uppercase tracking-wide text-slate-500">Label</span>
                    <input
                      :value="displayLabelFor(option)"
                      type="text"
                      class="mt-1 min-h-10 w-full rounded-lg border border-slate-200 bg-white px-3 text-base font-semibold text-slate-900 focus:outline-none focus:ring-4 focus:ring-cyan-200"
                      @input="updateCardLabel(option, $event.target.value)"
                    />
                  </label>
                </div>
                <div class="mt-3 flex flex-col gap-2 sm:flex-row">
                  <label class="inline-flex min-h-10 cursor-pointer items-center justify-center rounded-lg border border-slate-200 bg-white px-3 text-sm font-bold text-slate-800 shadow-sm transition hover:border-slate-300">
                    Upload image
                    <input
                      type="file"
                      accept="image/*"
                      class="sr-only"
                      @change="updateCardImage(option, $event)"
                    />
                  </label>
                  <button
                    type="button"
                    class="inline-flex min-h-10 items-center justify-center rounded-lg border border-slate-200 bg-white px-3 text-sm font-bold text-slate-700 shadow-sm transition hover:border-slate-300 disabled:cursor-not-allowed disabled:opacity-40"
                    :disabled="!displayImageFor(option)"
                    @click="resetCardImage(option)"
                  >
                    Use emoji
                  </button>
                </div>
              </div>
            </div>
          </div>
          <p
            v-if="settingsMessage"
            class="mt-3 rounded-lg border border-emerald-200 bg-emerald-50 px-4 py-3 text-sm font-bold text-emerald-800"
            role="status"
            aria-live="polite"
          >
            {{ settingsMessage }}
          </p>
        </div>

        <section class="flex flex-1 flex-col rounded-lg border border-slate-200 bg-white p-4 shadow-soft sm:p-6">
          <div class="flex flex-col gap-2 border-b border-slate-100 pb-4 sm:flex-row sm:items-end sm:justify-between">
            <div>
              <p class="text-sm font-semibold text-cyan-700">{{ isComplete ? 'Message ready' : 'Select one' }}</p>
              <h2 class="text-2xl font-bold text-slate-950 sm:text-3xl">
                {{ isComplete ? 'Use this phrase' : currentNode.prompt }}
              </h2>
            </div>
            <div class="flex flex-wrap items-center gap-3">
              <button
                v-if="selectedPath.length === flowStepCount - 1 && !isComplete"
                type="button"
                class="inline-flex min-h-11 items-center justify-center gap-2 rounded-lg border border-slate-200 bg-white px-4 text-sm font-bold text-slate-800 shadow-sm transition hover:border-slate-300 focus:outline-none focus:ring-4 focus:ring-cyan-200"
                @click="resetFlow"
              >
                <Home class="h-4 w-4" />
                First page
              </button>
              <p class="text-sm font-medium text-slate-500">{{ visibleOptions.length || 1 }} choice{{ visibleOptions.length === 1 ? '' : 's' }}</p>
            </div>
          </div>

          <div v-if="!isComplete" class="grid flex-1 auto-rows-fr gap-3 pt-5 sm:grid-cols-2 lg:grid-cols-3 2xl:grid-cols-4">
            <button
              v-for="(option, index) in visibleOptions"
              :key="option.id"
              type="button"
              class="group grid min-h-28 touch-manipulation grid-rows-[1fr_auto] rounded-lg border p-3 text-left shadow-lg transition hover:-translate-y-0.5 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-cyan-200 sm:min-h-36"
              :class="toneClasses[option.tone]"
              @click="chooseOption(index)"
            >
              <img
                v-if="displayImageFor(option)"
                :src="displayImageFor(option)"
                :alt="displayLabelFor(option)"
                class="mx-auto h-full max-h-24 w-full rounded-md object-cover transition group-hover:scale-105 sm:max-h-28"
              />
              <span
                v-else
                class="flex items-center justify-center text-7xl leading-none transition group-hover:scale-105 sm:text-8xl"
                aria-hidden="true"
              >
                {{ visualCueFor(option) }}
              </span>
              <span class="mt-2 block text-xl font-bold leading-tight sm:text-3xl">{{ displayLabelFor(option) }}</span>
            </button>
          </div>

          <div v-else class="flex flex-1 flex-col items-center justify-center gap-5 py-12 text-center">
            <div class="rounded-full bg-emerald-100 p-5 text-emerald-700">
              <Check class="h-12 w-12" />
            </div>
            <p class="max-w-2xl text-5xl font-bold leading-tight text-slate-950">{{ phrase }}</p>
            <div class="grid w-full max-w-2xl gap-4 sm:grid-cols-2">
              <button
                type="button"
                class="grid min-h-40 touch-manipulation grid-rows-[1fr_auto] rounded-lg border border-slate-900 bg-slate-950 p-5 text-left text-white shadow-lg shadow-slate-300 transition hover:-translate-y-0.5 hover:bg-slate-800 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-cyan-200 sm:min-h-48"
                @click="speakPhrase"
              >
                <span class="flex items-center justify-center text-8xl leading-none sm:text-9xl" aria-hidden="true">🔈</span>
                <span class="text-3xl font-bold leading-tight sm:text-4xl">Speak</span>
              </button>
              <button
                type="button"
                class="grid min-h-40 touch-manipulation grid-rows-[1fr_auto] rounded-lg border p-5 text-left shadow-lg transition hover:-translate-y-0.5 hover:shadow-xl focus:outline-none focus:ring-4 focus:ring-cyan-200 sm:min-h-48"
                :class="toneClasses.emerald"
                @click="resetFlow"
              >
                <span class="relative mx-auto flex h-24 w-24 items-center justify-center sm:h-28 sm:w-28" aria-hidden="true">
                  <Home class="h-20 w-20 sm:h-24 sm:w-24" stroke-width="1.7" />
                  <span class="absolute -left-1 top-2 flex h-10 w-10 items-center justify-center rounded-full bg-emerald-100 text-emerald-900 shadow-sm sm:h-12 sm:w-12">
                    <ArrowLeft class="h-6 w-6 sm:h-7 sm:w-7" stroke-width="2.3" />
                  </span>
                </span>
                <span class="text-3xl font-bold leading-tight sm:text-4xl">Back home</span>
              </button>
            </div>
          </div>
        </section>
      </div>
    </section>
  </main>
</template>
  'no-ice': '🚫',
  nuggets: '🍗',
  pillow: '🛏️',
  small: '➖',
  vanilla: '🍦',
