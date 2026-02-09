import React, { useState, useEffect, useRef } from 'react';
import { 
  Beaker, 
  Flame, 
  Zap, 
  Activity, 
  Droplets, 
  Dna, 
  Trophy, 
  Star, 
  ArrowRight, 
  Home, 
  Volume2, 
  VolumeX,
  Play,
  CheckCircle,
  XCircle,
  Gamepad2,
  Brain,
  Timer,
  RefreshCw,
  User,
  Award,
  Crown,
  Lock,
  Sparkles,
  GraduationCap,
  School as SchoolIcon,
  FlaskConical,
  ThumbsUp,
  ThumbsDown,
  Puzzle,
  LayoutGrid,
  Search,
  Microscope
} from 'lucide-react';

// ==========================================
// ✏️ منطقة تعديل البيانات (اكتبي الأسماء هنا)
// ==========================================
const SCHOOL_INFO = {
  teacher: "....................", 
  principal: "....................", 
  school: "...................."     
};

// ==========================================
// 📚 البيانات التعليمية والأسئلة
// ==========================================

const CURRICULUM_DATA = [
  {
    id: 1,
    title: "الطاقة والتغيرات الكيميائية",
    icon: <Flame className="w-8 h-8" />,
    color: "from-rose-400 to-pink-500",
    questions: [
      { q: "وحدة قياس الطاقة الحرارية في النظام الدولي؟", options: ["الجول", "الكالوري", "الدرجة", "الفولت"], correct: 0 },
      { q: "تفاعل تنتقل فيه الحرارة من النظام للمحيط؟", options: ["ماص", "طارد", "متزن", "لا شيء"], correct: 1 },
      { q: "الحرارة اللازمة لرفع 1جم من الماء درجة واحدة؟", options: ["الحرارة النوعية", "السعر", "الجول", "المحتوى"], correct: 1 }
    ]
  },
  {
    id: 2,
    title: "سرعة التفاعلات الكيميائية",
    icon: <Zap className="w-8 h-8" />,
    color: "from-amber-400 to-orange-500",
    questions: [
      { q: "أي مما يلي لا يؤثر في سرعة التفاعل؟", options: ["التركيز", "الحرارة", "مساحة السطح", "شكل الوعاء"], correct: 3 },
      { q: "مادة تزيد سرعة التفاعل ولا تستهلك؟", options: ["المتفاعلة", "المحفز", "المثبط", "الناتج"], correct: 1 }
    ]
  },
  {
    id: 3,
    title: "الاتزان الكيميائي",
    icon: <Activity className="w-8 h-8" />,
    color: "from-emerald-400 to-teal-500",
    questions: [
      { q: "تساوي سرعة التفاعل الأمامي والعكسي يسمى:", options: ["الاتزان", "التوقف", "التام", "اللحظي"], correct: 0 },
      { q: "سحب الحرارة في تفاعل طارد يزيح الاتزان نحو:", options: ["النواتج", "المتفاعلات", "لا يتأثر", "يتوقف"], correct: 0 }
    ]
  },
  {
    id: 4,
    title: "الهيدروكربونات",
    icon: <Droplets className="w-8 h-8" />,
    color: "from-blue-400 to-indigo-500",
    questions: [
      { q: "أبسط الهيدروكربونات هو:", options: ["الميثان", "الإيثان", "البروبان", "البنزين"], correct: 0 },
      { q: "الصيغة العامة للألكانات:", options: ["CnH2n+2", "CnH2n", "CnH2n-2", "CnH2n+1"], correct: 0 }
    ]
  },
  {
    id: 5,
    title: "مشتقات المركبات الهيدروكربونية",
    icon: <Beaker className="w-8 h-8" />,
    color: "from-violet-400 to-purple-500",
    questions: [
      { q: "المجموعة الوظيفية في الكحولات:", options: ["الهيدروكسيل", "الأمين", "الكربونيل", "الإيثر"], correct: 0 },
      { q: "مسؤولة عن روائح الفواكه:", options: ["الإسترات", "الأمينات", "هاليدات الألكيل", "الأحماض"], correct: 0 }
    ]
  },
  {
    id: 6,
    title: "المركبات العضوية الحيوية",
    icon: <Dna className="w-8 h-8" />,
    color: "from-pink-400 to-rose-500",
    questions: [
      { q: "وحدة بناء البروتينات:", options: ["الأحماض الأمينية", "السكريات", "الدهون", "النيوكليوتيدات"], correct: 0 },
      { q: "الإنزيمات تعتبر من:", options: ["البروتينات", "الكربوهيدرات", "الدهون", "الفيتامينات"], correct: 0 }
    ]
  }
];

// ==========================================
// 🎮 بيانات الألعاب
// ==========================================

const MEMORY_CARDS_DATA = [
  { id: 1, content: "طارد للحرارة", matchId: 101 }, { id: 101, content: "ينتج حرارة", matchId: 1 },
  { id: 2, content: "المحفز", matchId: 102 }, { id: 102, content: "يزيد السرعة", matchId: 2 },
  { id: 3, content: "CH₄", matchId: 103 }, { id: 103, content: "الميثان", matchId: 3 },
  { id: 4, content: "pH < 7", matchId: 104 }, { id: 104, content: "حمض", matchId: 4 },
];

const TRUE_FALSE_DATA = [
  { text: "الميثان هو أبسط الهيدروكربونات", isTrue: true },
  { text: "الإنزيمات هي نوع من السكريات", isTrue: false }, 
  { text: "التفاعل الماص للحرارة يحتاج طاقة", isTrue: true },
  { text: "المحفز يستهلك أثناء التفاعل", isTrue: false },
  { text: "pH = 7 يعتبر متعادل", isTrue: true },
  { text: "الألكانات تحتوي روابط ثنائية", isTrue: false },
];

const GUESS_WORD_DATA = [
  { word: "ألكان", hint: "هيدروكربون مشبع روابطه أحادية" },
  { word: "جول", hint: "وحدة قياس الطاقة" },
  { word: "إستر", hint: "مركب مسؤول عن رائحة الفواكه" },
  { word: "بروتين", hint: "بوليمر يتكون من أحماض أمينية" },
];

const SORT_GAME_DATA = [
  { item: "احتراق الخشب", type: "طارد" },
  { item: "انصهار الجليد", type: "ماص" },
  { item: "تبخر الماء", type: "ماص" },
  { item: "انفجار الألعاب النارية", type: "طارد" },
  { item: "طهي الطعام", type: "ماص" },
  { item: "تجمد الماء", type: "طارد" },
];

const FORMULA_DATA = [
  { name: "ملح الطعام", correct: "NaCl", options: ["NaCl", "H2O", "HCl"] },
  { name: "الماء", correct: "H2O", options: ["H2O2", "HO", "H2O"] },
  { name: "حمض الهيدروكلوريك", correct: "HCl", options: ["HCl", "H2SO4", "HF"] },
  { name: "الإيثان", correct: "C2H6", options: ["CH4", "C2H6", "C3H8"] },
];

// بيانات لعبة الصور (الرسم والاسم)
const ORGANIC_GAME_DATA = [
  { type: 'benzene', correct: "بنزين عطري", wrong: "هكسان حلقي" },
  { type: 'methane', correct: "ميثان", wrong: "إيثان" },
  { type: 'ethene', correct: "إيثين", wrong: "إيثاين" },
  { type: 'cyclohexane', correct: "هكسان حلقي", wrong: "بنزين عطري" },
  { type: 'ethanol', correct: "إيثانول", wrong: "إيثيل أمين" },
];

const BADGES = [
  { id: 'first_win', title: 'بداية موفقة', icon: <Star />, desc: 'أكملي أول نشاط', condition: (s) => s >= 10 },
  { id: 'smart_mind', title: 'عقل ذكي', icon: <Brain />, desc: 'اجمعي 50 نقطة', condition: (s) => s >= 50 },
  { id: 'game_master', title: 'سيدة الألعاب', icon: <Gamepad2 />, desc: 'العب 3 العاب مختلفة', condition: (s, g) => g >= 3 },
  { id: 'pro_chemist', title: 'بروفيسورة', icon: <Crown />, desc: 'اجمعي 100 نقطة', condition: (s) => s >= 100 },
];

const AVATARS = [
  { id: 1, bg: 'bg-pink-400', emoji: '👩‍🔬' },
  { id: 2, bg: 'bg-purple-400', emoji: '🦸‍♀️' },
  { id: 3, bg: 'bg-indigo-400', emoji: '🧙‍♀️' },
  { id: 4, bg: 'bg-rose-400', emoji: '👸' },
];

// ==========================================
// 🧪 مكونات الرسومات (SVG)
// ==========================================

const MoleculeViewer = ({ type }) => {
  return (
    <div className="w-64 h-64 mx-auto bg-white border-4 border-gray-100 rounded-3xl flex items-center justify-center p-4 shadow-inner mb-6">
      <svg viewBox="0 0 200 200" className="w-full h-full">
        {type === 'benzene' && (
          <g transform="translate(100,100) scale(1.5)">
             <path d="M0,-50 L43.3,-25 L43.3,25 L0,50 L-43.3,25 L-43.3,-25 Z" fill="none" stroke="#333" strokeWidth="4" />
             <circle cx="0" cy="0" r="25" fill="none" stroke="#333" strokeWidth="3" />
             <text x="0" y="80" textAnchor="middle" fontSize="14" fill="#666">C₆H₆</text>
          </g>
        )}
        {type === 'cyclohexane' && (
          <g transform="translate(100,100) scale(1.5)">
             <path d="M0,-50 L43.3,-25 L43.3,25 L0,50 L-43.3,25 L-43.3,-25 Z" fill="none" stroke="#333" strokeWidth="4" />
             <text x="0" y="80" textAnchor="middle" fontSize="14" fill="#666">C₆H₁₂</text>
          </g>
        )}
        {type === 'methane' && (
          <g transform="translate(100,100)">
             <line x1="0" y1="0" x2="0" y2="-60" stroke="#333" strokeWidth="4" />
             <line x1="0" y1="0" x2="55" y2="40" stroke="#333" strokeWidth="4" />
             <line x1="0" y1="0" x2="-55" y2="40" stroke="#333" strokeWidth="4" />
             <line x1="0" y1="0" x2="0" y2="10" stroke="#333" strokeWidth="4" /> 
             <circle cx="0" cy="0" r="15" fill="#333" /> 
             <circle cx="0" cy="-60" r="10" fill="#E11D48" /> 
             <circle cx="55" cy="40" r="10" fill="#E11D48" /> 
             <circle cx="-55" cy="40" r="10" fill="#E11D48" /> 
             <text x="0" y="80" textAnchor="middle" fontSize="20" fill="#666">CH₄</text>
          </g>
        )}
        {type === 'ethene' && (
          <g transform="translate(100,100)">
             <line x1="-30" y1="0" x2="30" y2="0" stroke="#333" strokeWidth="8" /> 
             <line x1="-30" y1="0" x2="-60" y2="-50" stroke="#333" strokeWidth="4" />
             <line x1="-30" y1="0" x2="-60" y2="50" stroke="#333" strokeWidth="4" />
             <line x1="30" y1="0" x2="60" y2="-50" stroke="#333" strokeWidth="4" />
             <line x1="30" y1="0" x2="60" y2="50" stroke="#333" strokeWidth="4" />
             <circle cx="-30" cy="0" r="12" fill="#333" /> 
             <circle cx="30" cy="0" r="12" fill="#333" /> 
             <circle cx="-60" cy="-50" r="8" fill="#E11D48" /> 
             <circle cx="-60" cy="50" r="8" fill="#E11D48" /> 
             <circle cx="60" cy="-50" r="8" fill="#E11D48" /> 
             <circle cx="60" cy="50" r="8" fill="#E11D48" /> 
             <text x="0" y="80" textAnchor="middle" fontSize="20" fill="#666">C₂H₄</text>
          </g>
        )}
        {type === 'ethanol' && (
          <g transform="translate(100,100)">
             <line x1="-40" y1="0" x2="10" y2="0" stroke="#333" strokeWidth="4" />
             <line x1="10" y1="0" x2="40" y2="-30" stroke="#333" strokeWidth="4" />
             <circle cx="-40" cy="0" r="12" fill="#333" /> 
             <circle cx="10" cy="0" r="12" fill="#333" /> 
             <circle cx="40" cy="-30" r="12" fill="#E11D48" /> 
             <text x="65" y="-35" fontSize="16" fontWeight="bold">H</text>
             <text x="-40" y="40" textAnchor="middle" fontSize="14" fill="#666">CH₃-CH₂-OH</text>
          </g>
        )}
      </svg>
    </div>
  );
};

// ==========================================
// 🎆 المكونات الأساسية (Main App)
// ==========================================

const Confetti = ({ active }) => {
  const canvasRef = useRef(null);
  useEffect(() => {
    if (!active) return;
    const canvas = canvasRef.current;
    const ctx = canvas.getContext('2d');
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    let particles = [];
    const colors = ['#FF69B4', '#FFD700', '#00FFFF', '#FF4500', '#ADFF2F'];
    for (let i = 0; i < 150; i++) particles.push({ x: canvas.width/2, y: canvas.height/2, r: Math.random()*6+2, dx: (Math.random()-0.5)*15, dy: (Math.random()-0.5)*15, color: colors[Math.floor(Math.random()*colors.length)], life: 100 });
    const animate = () => {
      if (!active) return;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      particles.forEach((p, i) => { p.x += p.dx; p.y += p.dy; p.life -= 1.5; p.dy += 0.2; ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI*2); ctx.fillStyle = p.color; ctx.fill(); if (p.life <= 0) particles.splice(i, 1); });
      if (particles.length > 0) requestAnimationFrame(animate);
    };
    animate();
  }, [active]);
  if (!active) return null;
  return <canvas ref={canvasRef} className="fixed inset-0 pointer-events-none z-50" />;
};

export default function ChemistryApp() {
  const [screen, setScreen] = useState('welcome');
  const [subScreen, setSubScreen] = useState(''); 
  
  // States
  const [score, setScore] = useState(0); 
  const [totalScore, setTotalScore] = useState(0); 
  const [gamesPlayedCount, setGamesPlayedCount] = useState(0);
  const [soundEnabled, setSoundEnabled] = useState(true);
  const [showConfetti, setShowConfetti] = useState(false);
  const [selectedAvatar, setSelectedAvatar] = useState(AVATARS[0]);
  const [unlockedBadges, setUnlockedBadges] = useState([]);

  // Game Specific States
  const [memoryCards, setMemoryCards] = useState([]);
  const [memoryFlipped, setMemoryFlipped] = useState([]);
  const [memoryMatched, setMemoryMatched] = useState([]);
  const [tfIndex, setTfIndex] = useState(0);
  const [tfTimer, setTfTimer] = useState(10);
  const [tfGameActive, setTfGameActive] = useState(false);
  const [wordIndex, setWordIndex] = useState(0);
  const [guessedLetters, setGuessedLetters] = useState([]);
  const [wordGameStatus, setWordGameStatus] = useState('playing'); 
  const [sortIndex, setSortIndex] = useState(0);
  const [sortFeedback, setSortFeedback] = useState(null);
  const [formulaIndex, setFormulaIndex] = useState(0);
  const [organicIndex, setOrganicIndex] = useState(0);
  const [activeChapter, setActiveChapter] = useState(null);
  const [currentQIndex, setCurrentQIndex] = useState(0);
  const [lastAnswerStatus, setLastAnswerStatus] = useState(null);

  // 🔊 Audio Logic (Enhanced Arabic)
  const playSound = (type) => {
    if (!soundEnabled) return;
    const synth = window.speechSynthesis;
    let text = "";
    
    if (type === 'correct') {
        const phrases = [
            "أَحْسَنْتِ، إِجَابَةٌ رَائِعَة", 
            "مُمْتَازَة", 
            "بَارَكَ اللَّهُ فِيكِ"
        ];
        text = phrases[Math.floor(Math.random() * phrases.length)];
    }
    
    if (type === 'wrong') text = "حَاوِلِي مَرَّةً أُخْرَى"; 
    if (type === 'win') text = "أَنْتِ مُذْهِلَة، مُبَارَكٌ لَكِ"; 
    if (type === 'badge') text = "مُبَارَك، لَقَدْ حَصَلْتِ عَلَى وِسَامٍ جَدِيد"; 

    if (text) {
      synth.cancel();
      const u = new SpeechSynthesisUtterance(text);
      u.lang = 'ar-SA';
      u.rate = 0.9;
      u.pitch = 1.1;
      synth.speak(u);
    }
  };

  useEffect(() => {
    let newBadges = [...unlockedBadges];
    let added = false;
    BADGES.forEach(b => {
      if (!newBadges.includes(b.id) && b.condition(totalScore, gamesPlayedCount)) {
        newBadges.push(b.id);
        added = true;
      }
    });
    if (added) {
      setUnlockedBadges(newBadges);
      playSound('badge');
      setShowConfetti(true);
      setTimeout(() => setShowConfetti(false), 3000);
    }
  }, [totalScore, gamesPlayedCount]);

  const addScore = (points) => {
    setScore(s => s + points);
    setTotalScore(s => s + points);
  };

  const finishGame = () => {
    setGamesPlayedCount(prev => prev + 1);
    playSound('win');
    setShowConfetti(true);
    setTimeout(() => {
        setShowConfetti(false);
        setSubScreen('game_result');
    }, 2000);
  };

  // 🎮 Game Logic Functions
  const startMemory = () => {
    const shuffled = [...MEMORY_CARDS_DATA].sort(() => Math.random() - 0.5).map(c => ({ ...c, uid: Math.random() }));
    setMemoryCards(shuffled);
    setMemoryFlipped([]);
    setMemoryMatched([]);
    setScore(0);
    setSubScreen('memory');
  };

  const handleMemoryClick = (idx) => {
    if (memoryFlipped.length === 2 || memoryFlipped.includes(idx) || memoryMatched.includes(memoryCards[idx].id)) return;
    const newFlipped = [...memoryFlipped, idx];
    setMemoryFlipped(newFlipped);
    if (newFlipped.length === 2) {
      const c1 = memoryCards[newFlipped[0]];
      const c2 = memoryCards[newFlipped[1]];
      if (c1.matchId === c2.id) {
        setTimeout(() => {
          setMemoryMatched(prev => [...prev, c1.id, c2.id]);
          setMemoryFlipped([]);
          addScore(10);
          playSound('correct');
          if (memoryMatched.length + 2 === memoryCards.length) finishGame();
        }, 800);
      } else {
        setTimeout(() => setMemoryFlipped([]), 1000);
      }
    }
  };

  const startTrueFalse = () => {
    setTfIndex(0);
    setScore(0);
    setTfTimer(10);
    setTfGameActive(true);
    setSubScreen('truefalse');
  };

  useEffect(() => {
    let interval;
    if (subScreen === 'truefalse' && tfGameActive && tfTimer > 0) {
      interval = setInterval(() => setTfTimer(t => t - 1), 1000);
    } else if (tfTimer === 0 && subScreen === 'truefalse' && tfGameActive) {
      handleTfAnswer(null); 
    }
    return () => clearInterval(interval);
  }, [subScreen, tfTimer, tfGameActive]);

  const handleTfAnswer = (userChoice) => {
    const current = TRUE_FALSE_DATA[tfIndex];
    let correct = false;
    if (userChoice === null) correct = false; 
    else correct = (userChoice === current.isTrue);
    if (correct) { addScore(5); playSound('correct'); } else { playSound('wrong'); }
    if (tfIndex < TRUE_FALSE_DATA.length - 1) { setTfIndex(prev => prev + 1); setTfTimer(10); } 
    else { setTfGameActive(false); finishGame(); }
  };

  const startGuessWord = () => {
    setWordIndex(0);
    setScore(0);
    setGuessedLetters([]);
    setWordGameStatus('playing');
    setSubScreen('guessword');
  };

  const handleLetterClick = (letter) => {
    if (wordGameStatus !== 'playing') return;
    if (guessedLetters.includes(letter)) return;
    setGuessedLetters([...guessedLetters, letter]);
    const currentWord = GUESS_WORD_DATA[wordIndex].word;
    if (!currentWord.includes(letter)) { playSound('wrong'); } else {
       const allGuessed = currentWord.split('').every(char => [...guessedLetters, letter].includes(char));
       if (allGuessed) {
         addScore(20); playSound('correct');
         setTimeout(() => {
           if (wordIndex < GUESS_WORD_DATA.length - 1) { setWordIndex(prev => prev + 1); setGuessedLetters([]); } else { finishGame(); }
         }, 1000);
       }
    }
  };

  const startSortGame = () => {
    setSortIndex(0);
    setScore(0);
    setSubScreen('sort');
    setSortFeedback(null);
  };

  const handleSortChoice = (type) => {
    const item = SORT_GAME_DATA[sortIndex];
    if (item.type === type) { addScore(5); playSound('correct'); setSortFeedback('correct'); } else { playSound('wrong'); setSortFeedback('wrong'); }
    setTimeout(() => {
      setSortFeedback(null);
      if (sortIndex < SORT_GAME_DATA.length - 1) { setSortIndex(prev => prev + 1); } else { finishGame(); }
    }, 800);
  };

  const startFormulaGame = () => {
    setFormulaIndex(0);
    setScore(0);
    setSubScreen('formula');
  };

  const handleFormulaChoice = (choice) => {
    const item = FORMULA_DATA[formulaIndex];
    if (choice === item.correct) { addScore(10); playSound('correct'); } else { playSound('wrong'); }
    setTimeout(() => {
      if (formulaIndex < FORMULA_DATA.length - 1) { setFormulaIndex(prev => prev + 1); } else { finishGame(); }
    }, 500);
  };

  const startOrganicGame = () => {
    setOrganicIndex(0);
    setScore(0);
    setSubScreen('organic');
  };

  const handleOrganicChoice = (isCorrect) => {
    if (isCorrect) {
      addScore(10); playSound('correct'); setShowConfetti(true); setTimeout(() => setShowConfetti(false), 1000);
    } else { playSound('wrong'); }
    setTimeout(() => {
      if (organicIndex < ORGANIC_GAME_DATA.length - 1) { setOrganicIndex(prev => prev + 1); } else { finishGame(); }
    }, 1000);
  };

  const startQuiz = (chapter) => {
    setActiveChapter(chapter);
    setCurrentQIndex(0);
    setScore(0);
    setScreen('quiz');
    setSubScreen('');
  };

  const handleQuizAnswer = (idx) => {
    const q = activeChapter.questions[currentQIndex];
    if (idx === q.correct) {
      addScore(10); setLastAnswerStatus('correct'); setShowConfetti(true); playSound('correct');
      setTimeout(() => {
        setShowConfetti(false);
        if (currentQIndex < activeChapter.questions.length - 1) { setCurrentQIndex(i => i + 1); setLastAnswerStatus(null); } 
        else { setSubScreen('quiz_result'); playSound('win'); setShowConfetti(true); }
      }, 1500);
    } else {
      setLastAnswerStatus('wrong'); playSound('wrong'); setTimeout(() => setLastAnswerStatus(null), 1000);
    }
  };

  // 🖌️ UI Components
  const GameCard = ({ title, icon, color, onClick, desc }) => (
    <button onClick={onClick} className="bg-white rounded-3xl p-6 shadow-lg border border-gray-100 hover:shadow-xl hover:-translate-y-1 transition-all text-right group relative overflow-hidden">
      <div className={`absolute top-0 right-0 w-24 h-24 ${color} opacity-10 rounded-bl-full -mr-4 -mt-4 transition-transform group-hover:scale-110`}></div>
      <div className={`w-14 h-14 ${color} text-white rounded-2xl flex items-center justify-center mb-4 shadow-md relative z-10`}>{icon}</div>
      <h3 className="font-bold text-gray-800 text-lg relative z-10">{title}</h3>
      <p className="text-gray-500 text-xs mt-1 relative z-10">{desc}</p>
      <div className="mt-4 flex items-center text-purple-600 text-sm font-bold opacity-0 group-hover:opacity-100 transition-opacity"><Play size={14} className="ml-1" /> العب الآن</div>
    </button>
  );

  return (
    <div className="min-h-screen bg-gray-50 font-sans text-right select-none" dir="rtl">
      <Confetti active={showConfetti} />
      
      {/* Header */}
      <div className="bg-white shadow-sm p-4 flex justify-between items-center sticky top-0 z-40">
        <div className="flex items-center gap-2">
           <div className="w-10 h-10 bg-gradient-to-tr from-pink-500 to-purple-500 rounded-full flex items-center justify-center text-white shadow-md"><Beaker size={20} /></div>
           <div><h1 className="text-lg font-bold text-gray-800 hidden md:block">منصة الكيمياء 2-2</h1><span className="md:hidden text-purple-600 font-bold text-sm">{totalScore} نقطة</span></div>
        </div>
        <div className="flex gap-2 items-center">
          <div className="hidden md:flex items-center gap-2 bg-pink-50 px-3 py-1 rounded-full border border-pink-100"><span className="text-sm font-bold text-purple-600">{totalScore} نقطة</span></div>
          <button onClick={() => setSoundEnabled(!soundEnabled)} className="p-2 rounded-full hover:bg-gray-100 text-gray-600">{soundEnabled ? <Volume2 size={20} /> : <VolumeX size={20} />}</button>
          <button onClick={() => {setScreen('menu'); setSubScreen('');}} className="p-2 rounded-full hover:bg-gray-100 text-gray-600"><Home size={20} /></button>
        </div>
      </div>

      <div className="container mx-auto px-4 py-8 max-w-4xl min-h-[85vh] flex flex-col justify-center">

        {/* 1. Welcome Screen */}
        {screen === 'welcome' && (
          <div className="text-center space-y-8 animate-fade-in">
            <div className="w-40 h-40 mx-auto bg-gradient-to-br from-pink-400 to-purple-500 rounded-full flex items-center justify-center shadow-xl animate-bounce-slow"><span className="text-6xl">{selectedAvatar.emoji}</span></div>
            <h2 className="text-4xl font-black text-gray-800">مرحباً بكِ في عالم<br/><span className="text-transparent bg-clip-text bg-gradient-to-r from-pink-500 to-purple-600">الكيمياء الممتعة</span></h2>
            <button onClick={() => setScreen('menu')} className="px-8 py-4 bg-gradient-to-r from-pink-500 to-purple-600 text-white rounded-full font-bold shadow-lg hover:shadow-pink-500/30 hover:-translate-y-1 transition-all text-lg flex items-center justify-center mx-auto gap-2"><span>ابدأ الرحلة</span> <ArrowRight /></button>
            <div className="mt-8 bg-white/70 backdrop-blur p-6 rounded-3xl border border-pink-100 max-w-2xl mx-auto grid grid-cols-1 md:grid-cols-3 gap-4">
               {[
                 { icon: <FlaskConical className="text-indigo-500"/>, label: "المعلمة", val: SCHOOL_INFO.teacher },
                 { icon: <SchoolIcon className="text-pink-500"/>, label: "المدرسة", val: SCHOOL_INFO.school },
                 { icon: <GraduationCap className="text-purple-500"/>, label: "المديرة", val: SCHOOL_INFO.principal },
               ].map((item, i) => (
                 <div key={i} className="flex flex-col items-center">
                    <div className="w-10 h-10 bg-gray-100 rounded-full flex items-center justify-center mb-2">{item.icon}</div>
                    <span className="text-xs text-gray-400 font-bold">{item.label}</span>
                    <span className="text-sm font-bold text-gray-800 border-b border-gray-200 pb-1">{item.val}</span>
                 </div>
               ))}
            </div>
          </div>
        )}

        {/* 2. Main Menu */}
        {screen === 'menu' && !subScreen && (
          <div className="space-y-6 animate-fade-in-up">
            <div className="flex justify-between items-end">
               <div><h2 className="text-3xl font-bold text-gray-800">القائمة الرئيسية</h2><p className="text-gray-500">ماذا تودين أن تفعلي اليوم؟</p></div>
               <button onClick={() => setScreen('profile')} className="bg-white text-purple-600 px-4 py-2 rounded-xl border border-purple-100 shadow-sm flex gap-2 items-center hover:bg-purple-50"><User size={18}/> ملفي</button>
            </div>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-6 h-64">
              <button onClick={() => setScreen('chapters')} className="bg-gradient-to-br from-pink-500 to-rose-600 rounded-3xl p-8 text-white text-right shadow-lg hover:shadow-pink-500/30 hover:-translate-y-1 transition-all relative overflow-hidden group">
                 <Beaker size={64} className="absolute -bottom-4 -left-4 opacity-20 group-hover:scale-110 transition-transform" />
                 <h3 className="text-2xl font-bold mb-2">الدراسة والاختبارات</h3>
                 <p className="opacity-90">تصفحي الفصول الستة واجمعي النقاط</p>
              </button>
              <button onClick={() => setScreen('games')} className="bg-gradient-to-br from-purple-500 to-indigo-600 rounded-3xl p-8 text-white text-right shadow-lg hover:shadow-purple-500/30 hover:-translate-y-1 transition-all relative overflow-hidden group">
                 <Gamepad2 size={64} className="absolute -bottom-4 -left-4 opacity-20 group-hover:scale-110 transition-transform" />
                 <h3 className="text-2xl font-bold mb-2">اللعب وتعلّم</h3>
                 <p className="opacity-90">6 ألعاب تفاعلية لتعزيز المهارات</p>
              </button>
            </div>
          </div>
        )}

        {/* 3. Games Menu */}
        {screen === 'games' && !subScreen && (
           <div className="space-y-6 animate-fade-in">
             <div className="flex items-center gap-2 mb-4">
               <button onClick={() => setScreen('menu')} className="bg-gray-100 p-2 rounded-full hover:bg-gray-200"><ArrowRight size={20}/></button>
               <h2 className="text-2xl font-bold text-gray-800">مركز الألعاب</h2>
             </div>
             <div className="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
                <GameCard title="الذاكرة الكيميائية" icon={<Brain/>} color="bg-pink-500" desc="طابقي المصطلحات" onClick={startMemory} />
                <GameCard title="صح أم خطأ" icon={<Timer/>} color="bg-orange-500" desc="تحدي السرعة" onClick={startTrueFalse} />
                <GameCard title="كلمة السر" icon={<Search/>} color="bg-purple-500" desc="خمني المصطلح" onClick={startGuessWord} />
                <GameCard title="المختبر الذكي" icon={<LayoutGrid/>} color="bg-blue-500" desc="صنفي التفاعلات" onClick={startSortGame} />
                <GameCard title="سيد الصيغ" icon={<FlaskConical/>} color="bg-teal-500" desc="اختاري الصيغة" onClick={startFormulaGame} />
                <GameCard title="تعرف على المركب" icon={<Microscope/>} color="bg-rose-500" desc="الرسم والاسم" onClick={startOrganicGame} />
             </div>
           </div>
        )}

        {/* --- GAME SCREENS --- */}

        {/* Game 1: Memory */}
        {subScreen === 'memory' && (
          <div className="animate-fade-in">
            <div className="flex justify-between items-center mb-6">
               <h3 className="font-bold text-gray-700 text-xl">الذاكرة الكيميائية</h3>
               <div className="bg-pink-100 text-pink-600 px-4 py-1 rounded-full font-bold">{score} نقطة</div>
            </div>
            <div className="grid grid-cols-4 gap-3 md:gap-4">
              {memoryCards.map((c, i) => {
                const flipped = memoryFlipped.includes(i) || memoryMatched.includes(c.id);
                const matched = memoryMatched.includes(c.id);
                return (
                  <div key={c.uid} onClick={() => handleMemoryClick(i)} className="aspect-square cursor-pointer relative perspective-1000">
                    <div className={`w-full h-full transition-all duration-500 transform-style-3d ${flipped ? 'rotate-y-180' : ''}`}>
                       <div className="absolute inset-0 backface-hidden bg-gradient-to-br from-indigo-500 to-purple-600 rounded-xl shadow-md flex items-center justify-center border-2 border-white"><Brain className="text-white opacity-50" /></div>
                       <div className={`absolute inset-0 backface-hidden rotate-y-180 rounded-xl shadow-lg flex items-center justify-center p-1 text-center border-2 select-none ${matched ? 'bg-green-50 border-green-400' : 'bg-white border-purple-200'}`}><span className={`text-xs md:text-sm font-bold ${matched ? 'text-green-700' : 'text-gray-800'}`}>{c.content}</span></div>
                    </div>
                  </div>
                )
              })}
            </div>
          </div>
        )}

        {/* Game 2: True/False */}
        {subScreen === 'truefalse' && (
           <div className="max-w-md mx-auto w-full animate-fade-in text-center">
              <div className="mb-6 flex justify-between items-center">
                 <div className="w-16 h-16 rounded-full border-4 border-orange-100 flex items-center justify-center font-bold text-2xl text-orange-500 shadow-sm">{tfTimer}</div>
                 <div className="text-xl font-bold text-gray-700">السؤال {tfIndex + 1}/{TRUE_FALSE_DATA.length}</div>
              </div>
              <div className="bg-white p-8 rounded-3xl shadow-xl border border-gray-100 mb-8 min-h-[200px] flex items-center justify-center"><h3 className="text-2xl font-bold text-gray-800 leading-relaxed">{TRUE_FALSE_DATA[tfIndex].text}</h3></div>
              <div className="grid grid-cols-2 gap-4">
                 <button onClick={() => handleTfAnswer(true)} className="bg-green-100 hover:bg-green-200 text-green-600 p-6 rounded-2xl flex flex-col items-center gap-2 transition-colors"><CheckCircle size={40} /> <span className="font-bold text-xl">صح</span></button>
                 <button onClick={() => handleTfAnswer(false)} className="bg-red-100 hover:bg-red-200 text-red-600 p-6 rounded-2xl flex flex-col items-center gap-2 transition-colors"><XCircle size={40} /> <span className="font-bold text-xl">خطأ</span></button>
              </div>
           </div>
        )}

        {/* Game 3: Guess Word */}
        {subScreen === 'guessword' && (
           <div className="animate-fade-in text-center max-w-lg mx-auto">
              <div className="bg-purple-100 text-purple-700 px-6 py-3 rounded-xl inline-block mb-8 font-bold">تلميح: {GUESS_WORD_DATA[wordIndex].hint}</div>
              <div className="flex gap-2 justify-center mb-8 dir-ltr">
                 {GUESS_WORD_DATA[wordIndex].word.split('').map((char, i) => (
                    <div key={i} className={`w-12 h-12 rounded-lg border-2 flex items-center justify-center text-2xl font-bold ${guessedLetters.includes(char) ? 'bg-white border-purple-500 text-purple-700' : 'bg-gray-100 border-gray-300 text-transparent'}`}>{guessedLetters.includes(char) ? char : '?'}</div>
                 ))}
              </div>
              <div className="grid grid-cols-7 gap-2">
                 {"ابتثجحخدذرزسشصضطظعغفقكلمنهوي".split('').map(char => (
                    <button key={char} disabled={guessedLetters.includes(char)} onClick={() => handleLetterClick(char)} className={`aspect-square rounded-lg font-bold transition-colors ${guessedLetters.includes(char) ? 'bg-gray-200 text-gray-400' : 'bg-white shadow border hover:bg-purple-50 text-gray-700'}`}>{char}</button>
                 ))}
              </div>
           </div>
        )}

        {/* Game 4: Sort Game */}
        {subScreen === 'sort' && (
           <div className="max-w-md mx-auto w-full animate-fade-in text-center relative">
              <h3 className="text-gray-500 mb-8 font-bold">صنفي التفاعل أو الحالة التالية:</h3>
              <div className={`bg-white p-12 rounded-3xl shadow-xl mb-8 border-2 transition-all transform duration-300 ${sortFeedback === 'correct' ? 'border-green-400 bg-green-50 scale-105' : sortFeedback === 'wrong' ? 'border-red-400 bg-red-50 shake' : 'border-gray-100'}`}><h2 className="text-3xl font-black text-gray-800">{SORT_GAME_DATA[sortIndex].item}</h2></div>
              <div className="flex gap-4 justify-center">
                 <button onClick={() => handleSortChoice('طارد')} className="flex-1 bg-gradient-to-r from-red-400 to-red-500 text-white p-4 rounded-2xl font-bold text-xl shadow-lg hover:shadow-red-500/30 hover:-translate-y-1 transition-all">طارد للحرارة 🔥</button>
                 <button onClick={() => handleSortChoice('ماص')} className="flex-1 bg-gradient-to-r from-blue-400 to-blue-500 text-white p-4 rounded-2xl font-bold text-xl shadow-lg hover:shadow-blue-500/30 hover:-translate-y-1 transition-all">ماص للحرارة ❄️</button>
              </div>
           </div>
        )}

        {/* Game 5: Formula Master */}
        {subScreen === 'formula' && (
           <div className="max-w-md mx-auto w-full animate-fade-in">
              <div className="bg-teal-600 text-white p-8 rounded-t-3xl text-center"><h2 className="text-2xl font-bold opacity-90">ما هي الصيغة الصحيحة لـ:</h2><h1 className="text-4xl font-black mt-2">{FORMULA_DATA[formulaIndex].name}</h1></div>
              <div className="bg-white p-6 rounded-b-3xl shadow-xl border border-gray-100">
                 <div className="grid gap-3">
                    {FORMULA_DATA[formulaIndex].options.map((opt, i) => (
                       <button key={i} onClick={() => handleFormulaChoice(opt)} className="w-full p-4 text-center border-2 border-gray-100 rounded-xl hover:border-teal-400 hover:bg-teal-50 font-mono text-xl font-bold text-gray-700 transition-all">{opt}</button>
                    ))}
                 </div>
              </div>
           </div>
        )}

        {/* Game 6: Organic Compound Recognition (NEW) */}
        {subScreen === 'organic' && (
          <div className="max-w-md mx-auto w-full animate-fade-in text-center">
             <div className="mb-6 flex justify-between items-center text-sm font-bold text-gray-500">
                <span className="bg-rose-100 text-rose-600 px-4 py-1 rounded-full">تعرف على المركب</span>
                <span>{organicIndex + 1}/{ORGANIC_GAME_DATA.length}</span>
             </div>
             <MoleculeViewer type={ORGANIC_GAME_DATA[organicIndex].type} />
             <h3 className="text-lg font-bold text-gray-700 mb-4">ما هو اسم هذا المركب؟</h3>
             <div className="grid grid-cols-2 gap-4">
                {[
                  { label: ORGANIC_GAME_DATA[organicIndex].correct, isCorrect: true },
                  { label: ORGANIC_GAME_DATA[organicIndex].wrong, isCorrect: false }
                ].sort(() => Math.random() - 0.5).map((opt, i) => (
                   <button key={i} onClick={() => handleOrganicChoice(opt.isCorrect)} className="bg-white border-2 border-gray-200 p-4 rounded-2xl text-lg font-bold text-gray-700 hover:border-rose-400 hover:bg-rose-50 hover:text-rose-700 transition-all shadow-sm active:scale-95">{opt.label}</button>
                ))}
             </div>
          </div>
        )}

        {/* Game Result Screen */}
        {subScreen === 'game_result' && (
           <div className="text-center animate-fade-in space-y-6">
              <div className="w-32 h-32 mx-auto bg-yellow-100 rounded-full flex items-center justify-center text-yellow-500 shadow-inner"><Trophy size={64} /></div>
              <h2 className="text-3xl font-black text-gray-800">أداء رائع!</h2>
              <p className="text-gray-500">لقد أتممت النشاط بنجاح</p>
              <div className="text-5xl font-black text-purple-600">{score} <span className="text-lg text-gray-400">نقطة</span></div>
              <div className="flex justify-center gap-4">
                 <button onClick={() => {setScreen('games'); setSubScreen('');}} className="bg-gray-200 text-gray-700 px-8 py-3 rounded-full font-bold hover:bg-gray-300">ألعاب أخرى</button>
                 <button onClick={() => setScreen('menu')} className="bg-purple-600 text-white px-8 py-3 rounded-full font-bold hover:bg-purple-700">القائمة</button>
              </div>
           </div>
        )}

        {/* 4. Chapters Screen */}
        {screen === 'chapters' && (
          <div className="space-y-6 animate-fade-in">
             <div className="flex items-center gap-2 mb-4">
               <button onClick={() => setScreen('menu')} className="bg-gray-100 p-2 rounded-full hover:bg-gray-200"><ArrowRight size={20}/></button>
               <h2 className="text-2xl font-bold text-gray-800">فصول المنهج</h2>
             </div>
             <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                {CURRICULUM_DATA.map(ch => (
                   <button key={ch.id} onClick={() => startQuiz(ch)} className="bg-white p-6 rounded-3xl shadow hover:shadow-xl transition-all border border-gray-100 text-right group relative overflow-hidden">
                      <div className={`absolute left-0 top-0 w-1 h-full bg-gradient-to-b ${ch.color}`}></div>
                      <div className={`w-12 h-12 rounded-xl bg-gradient-to-br ${ch.color} text-white flex items-center justify-center mb-4 shadow-md`}>{ch.icon}</div>
                      <h3 className="font-bold text-gray-800 text-lg group-hover:text-pink-600 transition-colors">{ch.title}</h3>
                      <p className="text-xs text-gray-400 mt-2">اختبار تفاعلي</p>
                   </button>
                ))}
             </div>
          </div>
        )}

        {/* 5. Quiz Screen */}
        {screen === 'quiz' && !subScreen && activeChapter && (
           <div className="max-w-2xl mx-auto w-full animate-fade-in">
              <div className="mb-6 flex justify-between items-center text-sm font-bold text-gray-500">
                 <span>{activeChapter.title}</span>
                 <span className="bg-pink-100 text-pink-600 px-3 py-1 rounded-full">{currentQIndex + 1}/{activeChapter.questions.length}</span>
              </div>
              <div className="bg-white rounded-3xl shadow-xl p-8 border border-gray-100 relative overflow-hidden">
                 {lastAnswerStatus === 'correct' && <div className="absolute inset-0 bg-green-50/90 flex items-center justify-center z-10"><div className="text-center"><CheckCircle size={64} className="text-green-500 mx-auto mb-2"/><p className="font-bold text-green-700">أحسنت!</p></div></div>}
                 {lastAnswerStatus === 'wrong' && <div className="absolute inset-0 bg-red-50/90 flex items-center justify-center z-10"><div className="text-center"><XCircle size={64} className="text-red-500 mx-auto mb-2"/><p className="font-bold text-red-700">حاول مرة أخرى</p></div></div>}
                 
                 <h3 className="text-2xl font-bold text-gray-800 mb-8 leading-relaxed">{activeChapter.questions[currentQIndex].q}</h3>
                 <div className="grid gap-3">
                    {activeChapter.questions[currentQIndex].options.map((opt, i) => (
                       <button key={i} onClick={() => handleQuizAnswer(i)} className="w-full text-right p-4 rounded-xl border-2 border-gray-100 hover:border-pink-300 hover:bg-pink-50 transition-all font-medium text-gray-700 flex items-center gap-3">
                          <div className="w-8 h-8 rounded-full bg-gray-100 flex items-center justify-center text-gray-500 text-sm font-bold">{['أ','ب','ج','د'][i]}</div>
                          {opt}
                       </button>
                    ))}
                 </div>
              </div>
           </div>
        )}

        {/* Quiz Result */}
        {subScreen === 'quiz_result' && (
           <div className="text-center animate-fade-in space-y-6">
              <div className="inline-block relative">
                 <div className="absolute inset-0 bg-yellow-200 rounded-full blur-2xl opacity-50 animate-pulse"></div>
                 <Star size={100} className="text-yellow-400 relative z-10 fill-yellow-400" />
              </div>
              <h2 className="text-3xl font-black text-gray-800">أتممتِ الفصل بنجاح!</h2>
              <div className="bg-white p-6 rounded-2xl shadow-sm border border-pink-100 inline-block min-w-[200px]">
                 <div className="text-gray-400 text-xs font-bold mb-1">النقاط المكتسبة</div>
                 <div className="text-4xl font-black text-pink-600">+{score}</div>
              </div>
              <div className="flex justify-center gap-4">
                 <button onClick={() => setScreen('chapters')} className="bg-pink-600 text-white px-8 py-3 rounded-full font-bold hover:bg-pink-700 shadow-lg hover:shadow-pink-500/30">فصل آخر</button>
                 <button onClick={() => setScreen('menu')} className="bg-gray-200 text-gray-700 px-8 py-3 rounded-full font-bold hover:bg-gray-300">القائمة</button>
              </div>
           </div>
        )}

        {/* 6. Profile Screen */}
        {screen === 'profile' && (
           <div className="animate-fade-in space-y-6">
              <div className="flex items-center gap-2 mb-4">
                 <button onClick={() => setScreen('menu')} className="bg-gray-100 p-2 rounded-full hover:bg-gray-200"><ArrowRight size={20}/></button>
                 <h2 className="text-2xl font-bold text-gray-800">ملفي الشخصي</h2>
              </div>
              <div className="bg-white rounded-3xl p-8 shadow-lg border border-gray-100 flex flex-col md:flex-row items-center gap-8">
                 <div className="text-center">
                    <div className={`w-32 h-32 ${selectedAvatar.bg} rounded-full flex items-center justify-center text-6xl shadow-xl mb-4`}>{selectedAvatar.emoji}</div>
                    <div className="font-bold text-gray-800 text-xl">{totalScore < 50 ? "مبتدئة" : totalScore < 100 ? "باحثة" : "عالمة"}</div>
                    <div className="text-purple-600 font-bold">{totalScore} نقطة</div>
                 </div>
                 <div className="flex-1">
                    <h4 className="font-bold text-gray-700 mb-4">شخصيتك:</h4>
                    <div className="flex gap-3 justify-center md:justify-start flex-wrap">
                       {AVATARS.map(a => <button key={a.id} onClick={() => setSelectedAvatar(a)} className={`w-12 h-12 rounded-full flex items-center justify-center text-2xl border-2 transition-all ${selectedAvatar.id === a.id ? 'border-purple-500 scale-110' : 'border-transparent hover:bg-gray-100'}`}>{a.emoji}</button>)}
                    </div>
                    <div className="mt-6">
                       <h4 className="font-bold text-gray-700 mb-4">الأوسمة:</h4>
                       <div className="flex gap-2 overflow-x-auto pb-2">
                          {BADGES.map(b => {
                             const unlocked = unlockedBadges.includes(b.id);
                             return (
                                <div key={b.id} className={`flex-shrink-0 w-24 p-3 rounded-xl border text-center ${unlocked ? 'bg-yellow-50 border-yellow-200' : 'bg-gray-50 border-gray-200 grayscale opacity-60'}`}>
                                   <div className="mx-auto w-8 h-8 flex items-center justify-center mb-1 text-yellow-600">{unlocked ? b.icon : <Lock size={16}/>}</div>
                                   <div className="text-[10px] font-bold text-gray-800">{b.title}</div>
                                </div>
                             )
                          })}
                       </div>
                    </div>
                 </div>
              </div>
           </div>
        )}

      </div>

      <style>{`
        @keyframes bounce-slow { 0%, 100% { transform: translateY(-5%); } 50% { transform: translateY(5%); } }
        .animate-bounce-slow { animation: bounce-slow 3s infinite ease-in-out; }
        .rotate-y-180 { transform: rotateY(180deg); }
        .transform-style-3d { transform-style: preserve-3d; }
        .backface-hidden { backface-visibility: hidden; }
        .perspective-1000 { perspective: 1000px; }
        @keyframes shake { 0%, 100% { transform: translateX(0); } 25% { transform: translateX(-5px); } 75% { transform: translateX(5px); } }
        .shake { animation: shake 0.4s ease-in-out; }
      `}</style>
    </div>
  );
}
