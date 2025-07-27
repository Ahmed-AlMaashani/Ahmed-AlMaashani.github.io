<!DOCTYPE html>
<html>

<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>JSFiddle udet25b8</title>

  <style>
    
  </style>

  
</head>
<body>
  <!DOCTYPE html>
<html lang="en" id="html" dir="rtl">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>نظام معلومات المبيدات الزراعية</title>
  <!-- Google Fonts -->
  <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;500;700&display=swap" rel="stylesheet">
  <!-- Tailwind CSS -->
  <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
  <style>
    body {
      font-family: 'Tajawal', sans-serif;
    }
    .ltr {
      font-family: system-ui, sans-serif;
    }
    .pest-card {
      transition: all 0.3s ease;
    }
    .pest-card:hover {
      transform: translateY(-4px);
      box-shadow: 0 10px 20px rgba(0,0,0,0.1);
    }
    .category-insecticide { @apply bg-green-100 border-green-300; }
    .category-fungicide { @apply bg-purple-100 border-purple-300; }
    .category-nematicide { @apply bg-red-100 border-red-300; }
    .category-biopesticide { @apply bg-blue-100 border-blue-300; }
  </style>
</head>
<body class="bg-gray-50 text-gray-800">

  <!-- Language Toggle -->
  <div class="flex justify-end mb-4 px-6 mt-4">
    <button onclick="setLang('ar')" class="mx-2 px-4 py-1 bg-blue-500 text-white rounded">عربي</button>
    <button onclick="setLang('en')" class="mx-2 px-4 py-1 bg-green-500 text-white rounded">English</button>
  </div>

  <div class="container mx-auto p-6 space-y-6 max-w-6xl">

    <!-- App Title -->
    <h1 id="appTitle" class="text-4xl font-bold text-center text-blue-700">نظام معلومات المبيدات الزراعية</h1>
    <p class="text-center text-gray-600" id="appSubtitle">ابحث أو اختر من القائمة</p>

    <!-- Search & Group Filter -->
    <div class="flex flex-col md:flex-row gap-4">
      <input type="text" id="searchInput" 
             placeholder="اكتب اسم المبيد، المادة الفعالة، أو المجموعة..." 
             class="flex-1 p-3 border-2 border-blue-300 rounded-lg shadow-sm focus:outline-none focus:border-blue-500 text-right"/>

      <!-- Group Dropdown -->
      <div class="relative">
        <button id="groupBtn" onclick="toggleGroupMenu()" 
                class="w-full md:w-48 p-3 bg-indigo-600 text-white rounded-lg shadow hover:bg-indigo-700">
          <span id="groupBtnText">اختر المجموعة</span>
        </button>
        <div id="groupMenu" class="hidden absolute z-10 mt-2 w-64 max-h-60 overflow-y-auto bg-white border rounded-lg shadow-lg">
          <div class="p-2 text-sm text-gray-500" id="noGroups">جاري التحميل...</div>
        </div>
      </div>
    </div>

    <!-- Results -->
    <div id="results" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mt-8"></div>

    <!-- Selected Details -->
    <div id="details" class="hidden mt-8 p-6 bg-white rounded-lg shadow-xl border-l-4 border-blue-500">
      <button onclick="closeDetails()" class="float-left text-red-500 text-3xl font-bold">&times;</button>
      <h2 id="detailName" class="text-2xl font-bold mb-4 text-gray-800"></h2>
      
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4 text-sm">
        <p><strong id="lblCategory"></strong>: <span id="detailCategory"></span></p>
        <p><strong id="lblActiveIngredient"></strong>: <span id="detailActiveIngredient"></span></p>
        <p><strong id="lblGroup"></strong>: <span id="detailGroup"></span></p>
        <p><strong id="lblSystemic"></strong>: <span id="detailSystemic"></span></p>
        <p><strong id="lblManufacturer"></strong>: <span id="detailManufacturer"></span></p>
        <p><strong id="lblTargetPests"></strong>: <span id="detailTargetPests"></span></p>
        <p><strong id="lblSafetyPeriod"></strong>: <span id="detailSafetyPeriod"></span> <span id="lblDays"></span></p>
        <p><strong id="lblDosage"></strong>: <span id="detailDosage"></span></p>
      </div>
      <div class="mt-4">
        <p><strong id="lblModeOfAction"></strong>:</p>
        <p id="detailModeOfAction" class="text-gray-700 mt-1 italic"></p>
      </div>
    </div>

    <!-- Calculator -->
    <div class="mt-8 p-6 bg-white rounded-lg shadow">
      <h2 id="calcTitle" class="text-2xl font-bold mb-4">حاسبة الجرعة</h2>
      <div class="space-y-3">
        <input type="number" id="waterVol" placeholder="كمية المياه (لتر)" 
               class="w-full p-3 border rounded-lg text-right"/>
        <input type="text" id="dosageRatio" placeholder="مثلاً: 0.25L/1000L"
               class="w-full p-3 border rounded-lg text-right"/>
        <button onclick="calculate()" class="bg-blue-500 hover:bg-blue-600 text-white px-6 py-3 rounded-lg text-lg w-full">
          <span id="calcBtn">احسب</span>
        </button>
        <p id="result" class="text-xl font-bold mt-3 text-green-600 text-center"></p>
      </div>
    </div>

    <!-- Copyright -->
    <div class="footer mt-12 text-center text-sm text-gray-500 border-t pt-4">
      © 2025 جميع الحقوق محفوظة - نظام معلومات المبيدات الزراعية<br>
      تم تطوير التطبيق بواسطة <strong>م. أحمد المعشني</strong>
    </div>
  </div>

  <script src="app.js"></script>
</body>
</html>

  <script>
    // Full Pesticide Data
const pesticides = [
  {
    name: "Avaunt",
    category: "Insecticide",
    activeIngredient: "Indoxacarb",
    group: "22A",
    systemic: "Non-Systemic",
    modeOfAction: "sodium channels blocker in the insect nervous system and the mode of entry is via the stomach and contact routes",
    manufacturer: "FMC, USA",
    targetPests: "Lepidopteran pests",
    safetyPeriod: 14,
    dosage: "0.25L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Actara",
    category: "Insecticide",
    activeIngredient: "Thiamethoxam",
    group: "4A",
    systemic: "Systemic",
    modeOfAction: "Contact & stomach action and is a Nicotinic acetylcholine receptor (nAChR) channel blocker, Inhibition of the enzyme ascorbate peroxidase",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Whitefly",
    safetyPeriod: 14,
    dosage: "0.4L-0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Benevia",
    category: "Insecticide",
    activeIngredient: "Cyantraniliprole",
    group: "28",
    systemic: "Systemic",
    modeOfAction: "Impairs the muscle function of the insects affects their feeding, movement & reproduction",
    manufacturer: "FMC, USA",
    targetPests: "Chewing & Sucking pests",
    safetyPeriod: 7,
    dosage: "0.75L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Cal-Ex Avance",
    category: "Insecticide",
    activeIngredient: "Abamectin 1.8%",
    group: "6",
    systemic: "Non-Systemic",
    modeOfAction: "Disrupts the central nervous system of pests",
    manufacturer: "FMC, USA",
    targetPests: "Mites, psylla, thrips",
    safetyPeriod: 14,
    dosage: "0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Coragen",
    category: "Insecticide",
    activeIngredient: "Chlorantraniliprole",
    group: "28",
    systemic: "Systemic",
    modeOfAction: "Chlorantraniliprole binds to a receptor in muscles called the ryanodine receptor and causes cells to leak calcium and paralyzed",
    manufacturer: "FMC, France",
    targetPests: "Larvae of Lepidopteran & Coleopteran",
    safetyPeriod: 7,
    dosage: "0.05L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Decis Expert",
    category: "Insecticide",
    activeIngredient: "Deltamethrin",
    group: "3A",
    systemic: "Non-Systemic",
    modeOfAction: "Preventing the transmission of nervous impulses along fibres, thereby disrupting nervous system, and death",
    manufacturer: "Bayer, Germany",
    targetPests: "Sucking & chewing insects",
    safetyPeriod: 14,
    dosage: "0.15L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Evisect",
    category: "Insecticide",
    activeIngredient: "Thiocyclam Hydrogen Oxalate",
    group: "14",
    systemic: "Systemic",
    modeOfAction: "Blocks the nervous system of the insect",
    manufacturer: "Sumitomo Chemical, Japan",
    targetPests: "Thrips, whiteflies, aphids",
    safetyPeriod: 14,
    dosage: "80g/100L",
    cropSafetyPeriods: {}
  },
  {
    name: "Floramite",
    category: "Insecticide",
    activeIngredient: "Bifenazate",
    group: "20D",
    systemic: "Non-Systemic",
    modeOfAction: "Disrupting the normal functioning of mite nerve cells, paralysis and death",
    manufacturer: "Chemtura, USA",
    targetPests: "Mites",
    safetyPeriod: 14,
    dosage: "0.4L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Green lambada",
    category: "Insecticide",
    activeIngredient: "Lambda-cyhalothrin",
    group: "3A",
    systemic: "Non-Systemic",
    modeOfAction: "Disrupting the gating mechanism of sodium channels that are involved in the generation and conduction of nerve impulses",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Aphids, whiteflies, thrips",
    safetyPeriod: 14,
    dosage: "0.25L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Karate Zeon",
    category: "Insecticide",
    activeIngredient: "Lambda Cyhalothrin 10%",
    group: "3A",
    systemic: "Non-Systemic",
    modeOfAction: "Disrupting the gating mechanism of sodium channels that are involved in the generation and conduction of nerve impulses",
    manufacturer: "Syngenta, Switzerland, made in Belgium",
    targetPests: "Whiteflies, thrips, Aphids",
    safetyPeriod: 14,
    dosage: "0.250L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Milbeknock",
    category: "Insecticide",
    activeIngredient: "Milbemectin",
    group: "6",
    systemic: "Non-Systemic",
    modeOfAction: "Glutamate-gated chloride channel (GluCl) activators",
    manufacturer: "Mitsui Chemicals, Japan",
    targetPests: "Mites",
    safetyPeriod: 14,
    dosage: "0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Mospilan",
    category: "Insecticide",
    activeIngredient: "Acetamiprid",
    group: "4A",
    systemic: "Systemic",
    modeOfAction: "The primary mechanism of acetamiprid toxicity against insects is due to its action at nicotinic cholinergic receptors. The unique nature of the neonicotinoids as insecticides is that the negatively charged cyano (or nitro) group will specifically interact with a cationic binding region that is unique to insects.",
    manufacturer: "Nippon soda or Summit Agro, Tokyo, Japan",
    targetPests: "Thrips, whiteflies, aphids, mealybug",
    safetyPeriod: 14,
    dosage: "0.4L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Movento",
    category: "Insecticide",
    activeIngredient: "Spirotetramat 10%",
    group: "23",
    systemic: "Systemic",
    modeOfAction: "inhibition of lipid biosynthesis",
    manufacturer: "Bayer, Germany",
    targetPests: "Whitefly, thrips, scale insects, Aphids",
    safetyPeriod: 14,
    dosage: "0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Ortus",
    category: "Insecticide",
    activeIngredient: "Fenpyroximate 5%",
    group: "21A",
    systemic: "Non-Systemic",
    modeOfAction: "Inhibiting mitochondrial complex I electron transport",
    manufacturer: "Nihon Nohyako, Japan",
    targetPests: "Mite",
    safetyPeriod: 7,
    dosage: "0.2L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Plesiva Pro",
    category: "Insecticide",
    activeIngredient: "Cyantraniliprole 13.5% / Abamectin 2.85%",
    group: "28/6",
    systemic: "Systemic",
    modeOfAction: "affects the Ryanodine receptors in the insect, leading to paralysis and death, Abamectin is a class of Avermectin that acts in a unique way on the neurotransmitter gamma-aminobutyric acid (GABA), which leads to blocking the transmission of nerve signals",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Whiteflies, Aphids, Thrips, Caterpillars, Leaf miners.",
    safetyPeriod: 7,
    dosage: "0.115L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Radiant",
    category: "Insecticide",
    activeIngredient: "Spinetoram 12%",
    group: "5",
    systemic: "Systemic",
    modeOfAction: "disruption of nicotinic/gamma amino butyric acid (GABA)-gated chloride channels",
    manufacturer: "Dow Agrosciences, France",
    targetPests: "Thrips, Lepidoptera larvae, psyllids",
    safetyPeriod: 7,
    dosage: "0.1L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Sivanto prime",
    category: "Insecticide",
    activeIngredient: "Flupyradifurone 20%",
    group: "4D",
    systemic: "Systemic",
    modeOfAction: "act on the central nervous system of target insect pests as an agonist of the nicotinic acetylcholine receptor (nAChR).",
    manufacturer: "Bayer, Germany",
    targetPests: "piercing, sucking insects",
    safetyPeriod: 7,
    dosage: "0.06L/100L",
    cropSafetyPeriods: {}
  },
  {
    name: "Starkle",
    category: "Insecticide",
    activeIngredient: "Dinotefuran 20%",
    group: "4A",
    systemic: "Systemic",
    modeOfAction: "Dinotefuran binds irreversibly to insect nicotinic receptors and mimics the effects of acetylcholine, resulting in continuous nerve stimulation, incoordination, tremors, and death of the insect",
    manufacturer: "Mitsui chemicals, Japan",
    targetPests: "aphids, whiteflies, thrips",
    safetyPeriod: 7,
    dosage: "100g/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Sumi-Alpha",
    category: "Insecticide",
    activeIngredient: "Esfenvalerate",
    group: "3A",
    systemic: "Contact",
    modeOfAction: "interferes with the insect's nerve cells, leading to paralysis and preventing the insect from performing normal functions",
    manufacturer: "Sumitomo chemical, Japan",
    targetPests: "moths, flies, beetles, and other insects",
    safetyPeriod: 14,
    dosage: "1L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Tracer",
    category: "Insecticide",
    activeIngredient: "Spinosad 48%",
    group: "5",
    systemic: "Systemic",
    modeOfAction: "by a neural mechanism. The spinosyns and spinosoids have a novel mode of action, primarily targeting binding sites on nicotinic acetylcholine receptors (nAChRs) of the insect nervous system",
    manufacturer: "Corteva Agriscience, France",
    targetPests: "Thrips, leafminers, caterpillars",
    safetyPeriod: 3,
    dosage: "0.08L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Trebon",
    category: "Insecticide",
    activeIngredient: "Etofenprox 20%",
    group: "3A",
    systemic: "Systemic",
    modeOfAction: "disturbs insect nervous systems",
    manufacturer: "Mitsui Chemicals, Japan",
    targetPests: "Thrips, Plant hopper, stink bug, caterpillar, aphid",
    safetyPeriod: 7,
    dosage: "0.050L/100L",
    cropSafetyPeriods: {}
  },
  {
    name: "Tripsol",
    category: "Insecticide",
    activeIngredient: "22.5 g/l Acrinathrin + 12.6 g/l Abamectin",
    group: "6/3A",
    systemic: "Non-Systemic",
    modeOfAction: "Acrinathrin is active by contact and ingestion, Abamectin stimulate the gamma-aminobutyric acid (GABA) system, a chemical 'transmitter' produced at nerve endings, which inhibits both nerve to nerve and nerve to muscle communication.",
    manufacturer: "FMC, USA",
    targetPests: "Thrips",
    safetyPeriod: 7,
    dosage: "0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Verimark 20%",
    category: "Insecticide",
    activeIngredient: "Cyantraniliprole 20%",
    group: "28",
    systemic: "Systemic",
    modeOfAction: "Cyantraniliprole is the member of bisamides class of insecticides. It is a ryanodine receptor (RyR) modulator which kills insects through unregulated activation of RyR",
    manufacturer: "FMC, USA",
    targetPests: "Lepidopteran pests (e.g., caterpillars, moths) and coleopteran pests (e.g., rootworms, weevils)",
    safetyPeriod: 7,
    dosage: "0.1L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Vertimec",
    category: "Insecticide",
    activeIngredient: "Abamectin 1.8%",
    group: "6",
    systemic: "Systemic",
    modeOfAction: "stimulate the gamma-aminobutyric acid (GABA) system, a chemical 'transmitter' produced at nerve endings, which inhibits both nerve to nerve and nerve to muscle communication",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "mites, leafminers",
    safetyPeriod: 14,
    dosage: "0.04L/100L",
    cropSafetyPeriods: {}
  },
  {
    name: "Voliam Flexi",
    category: "Insecticide",
    activeIngredient: "Chlorantraniliprole 100g/l + Thiamethoxam 200g/l",
    group: "28+4A",
    systemic: "Systemic",
    modeOfAction: "Chlorantraniliprole binds to a specific receptor in muscles called the ryanodine receptor. When chlorantraniliprole binds to this receptor, it causes muscle cells to leak calcium. The muscles stop working normally. The insect is paralyzed and dies thiamethoxam has both contact and stomach action and is a Nicotinic acetylcholine receptor (nAChR) channel blocker.",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Thrips, whitefly, leafminers, caterpillars, aphids",
    safetyPeriod: 14,
    dosage: "0.06L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Vydate",
    category: "Insecticide",
    activeIngredient: "Oxamyl 10%",
    group: "1A",
    systemic: "Systemic",
    modeOfAction: "cause toxicity by the irreversible inhibition of acetylcholinesterase (AChE), which results in the synaptic accumulation of acetylcholine (ACh), the ultimate toxicant",
    manufacturer: "DuPont, France",
    targetPests: "insects, mites and nematodes",
    safetyPeriod: 60,
    dosage: "Apply VYDATE at 30g-40g per 100m row",
    cropSafetyPeriods: {}
  },
  {
    name: "Velum prime",
    category: "Nematicide",
    activeIngredient: "Fluopyram 40%",
    group: "7",
    systemic: "Systemic",
    modeOfAction: "Fluopyram represents a new group of fungicide chemistry called pyridinyl ethylbenzimides that are SDHI's within the fungal mitochondrial chain, thus blocking electron transport. Similar fungicide active ingredients include boscalid and carboxin.",
    manufacturer: "Bayer, Germany",
    targetPests: "Root Knot nematodes",
    safetyPeriod: 14,
    dosage: "0.125L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Amistar top",
    category: "Fungicide",
    activeIngredient: "Difenoconazole + Azoxystrobin",
    group: "3+11",
    systemic: "Systemic",
    modeOfAction: "azoxystrobin: preventing the respiration of fungi due to the disruption of electron transport chain, preventing ATP synthesis, difenoconazole: It stops the development of fungi by interfering with the biosynthesis of sterols in cell membranes",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Septoria Leaf Spot, Powdery mildew, Alternaria solani, Downy mildew",
    safetyPeriod: 14,
    dosage: "0.07L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Armetil",
    category: "Fungicide",
    activeIngredient: "Metalaxyl 8% + Copper Oxychloride 67% (40% copper)",
    group: "E + M1",
    systemic: "Systemic",
    modeOfAction: "suppressing sporangial formation, mycelial growth and the establishment of new infections. Disrupts fungal nucleic acid synthesis - RNA ploymerase 1 + Copper kills spores by combining with sulphahydral groups of certain enzymes",
    manufacturer: "Industrias Químicas del Vallès, Spain",
    targetPests: "Alternaria solani, Downy mildew",
    safetyPeriod: 21,
    dosage: "1000g/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Azoshy",
    category: "Fungicide",
    activeIngredient: "Azoxystrobin 25%",
    group: "K",
    systemic: "Systemic",
    modeOfAction: "inhibiting mitochondrial respiration in fungi.",
    manufacturer: "Sc, Spain",
    targetPests: "Powdery mildew, Downy mildew, Anthracnose, Fusarium, Pythium, Alternaria, Phytophthora",
    safetyPeriod: 7,
    dosage: "0.1L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Curenox",
    category: "Fungicide",
    activeIngredient: "Cooper Oxychloride 84% (50% copper)",
    group: "M1",
    systemic: "Contact",
    modeOfAction: "It is a broad-spectrum contact fungicide with protective action. Copper because of its strong bonding affinity to amino acids and carboxyl groups, reacts with protein and acts as an enzyme inhibitor in target organisms. Copper kills spores by combining with sulphahydral groups of certain enzymes.",
    manufacturer: "Industrias Químicas del Vallés, Spain",
    targetPests: "late blight, Anthracnose, Downy mildew, Alternaria solani, Bacterial Spot",
    safetyPeriod: 15,
    dosage: "300g/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Equation Pro",
    category: "Fungicide",
    activeIngredient: "Famoxadone 22.5% + Cymoxanil 30%",
    group: "11 + cyanoacetamide-oxime",
    systemic: "Systemic",
    modeOfAction: "Cymoxanil inhibits synthesis of nucleic acids, amino acids, and other cellular processes in fungi lifecycle, famoxadone is inhibition of the fungal mitochondrial respiratory chain at Complex III, resulting in a decreased production of ATP by the fungal cell",
    manufacturer: "DuPont, France",
    targetPests: "Alternaria, Alternaria solani, Downy mildew, late blight",
    safetyPeriod: 16,
    dosage: "80g/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Kocide",
    category: "Fungicide",
    activeIngredient: "Copper Hydroxide 53.8% + Metalic Copper 35%",
    group: "Y",
    systemic: "Systemic",
    modeOfAction: "KOCIDE has Multisite activity, Cu+2 ions interfere with biomolecules due to its chelating properties, affect protein structures, function of enzymes, energy transport systems and membranes",
    manufacturer: "Cosaco LLC, USA",
    targetPests: "Alternaria, Anthracnose, Bacterial Blasts, Bacterial Blight and Spots, Blotches, Botrytis (gray mold), Citrus Canker (suppression), Downy Mildew and Powdery Mildew, Leaf Spot, Rots, Rust",
    safetyPeriod: 14,
    dosage: "1000g/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Luna Sensation",
    category: "Fungicide",
    activeIngredient: "Fluopyram 25% + Trifloxystrobin 25%",
    group: "FRAC 7 SDHI + FRAC 11 strobilurin/QoI",
    systemic: "Not specified",
    modeOfAction: "Trifloxystrobin works by interfering with respiration in plant pathogenic fungi, Fluopyram inhibits spore germination, germ tube elongation, mycelium growth and sporulation",
    manufacturer: "Bayer, Germany",
    targetPests: "leaf spot and blight",
    safetyPeriod: 14,
    dosage: "0.053L/100L",
    cropSafetyPeriods: {}
  },
  {
    name: "Miravis Duo",
    category: "Fungicide",
    activeIngredient: "Adepidyn 7.5% (Pydiflumetofen) + Difenoconazole 12.5%",
    group: "7 + 3",
    systemic: "Systemic",
    modeOfAction: "blocking the activity of succinate dehydrogenase (SDH), a universal enzyme expressed by all species harboring mitochondria",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Alternaria leafspot (A. alternata) Anthracnose (Colletotrichum acutatum) Blossom Blight Brown rot (Monilinia spp.) Brown rot/hull rot (Monilinia spp.) Powdery mildew (Podosphaera tridactyla, Sphaerotheca pannosa) Scab (Venturia carpophilia) Shot hole (Wilsonmyces carpophilus)",
    safetyPeriod: 14,
    dosage: "0.150L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Ortiva",
    category: "Fungicide",
    activeIngredient: "Azoxystrobin 200 g/L + Difenoconazole 125 g/L",
    group: "11 + 3",
    systemic: "Systemic",
    modeOfAction: "prevent the respiration of fungi due to the disruption of electron transport chain, preventing ATP synthesis, Difenoconazole: stops the development of fungi by interfering with the biosynthesis of sterols in cell membranes",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "botrytis, rust and downy mildew",
    safetyPeriod: 20,
    dosage: "0.7L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Previcur Energy",
    category: "Fungicide",
    activeIngredient: "propamocarb (530.0 g/L) + fosetyl (310.0 g/L)",
    group: "P07 + 28",
    systemic: "Systemic",
    modeOfAction: "Propamocarb has fungicidal activity only against oomycetes, fosetyl: inhibiting germination of spores and by blocking development of mycelium",
    manufacturer: "Bayer, Germany",
    targetPests: "Phytophthora spp. + Oomycete soil fungi, Pseudopeeonospora, Bremia, Peronospora, Pythium spp",
    safetyPeriod: 21,
    dosage: "0.5L/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Ridomail gold",
    category: "Fungicide",
    activeIngredient: "45.3% mefenoxam",
    group: "4 + M",
    systemic: "Systemic",
    modeOfAction: "Mefenoxam is a systemic fungicide with protective and curative properties. It is absorbed through the leaves, stems, and roots. It suppresses essential components of the disease so that the infection doesn't grow or spread.",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Pythium spp., Phytophthora spp., Peronospora parasitica",
    safetyPeriod: 21,
    dosage: "Xkg/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Score",
    category: "Fungicide",
    activeIngredient: "Difenoconazol 25%",
    group: "3",
    systemic: "Systemic",
    modeOfAction: "It stops the development of fungi by interfering with the biosynthesis of sterols in cell membranes",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Alternaria solani, Powdery mildew, leaf spots, Rust",
    safetyPeriod: 21,
    dosage: "0.05L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Top guard",
    category: "Fungicide",
    activeIngredient: "Flutriafol 11.8%",
    group: "3",
    systemic: "Systemic",
    modeOfAction: "inhibits the specific enzyme, C14-demethylase, a fungal cyctochrome P450, which plays a role in sterol production. Sterols are needed for fungal membrane structure and function and are essential for the development of functional cell walls.",
    manufacturer: "FMC, USA",
    targetPests: "Southern Corn Leaf Blight (Cochlibolus heterostrophus) Northern Corn Leaf Blight (Setosphaeria turcica) Rust, Common (Puccinia sorghi) Anthracnose Leaf Blight (Colletotrichum graminicola) Northern Corn Leaf Spot (Cochliobolus carbonum)",
    safetyPeriod: 21,
    dosage: "0.2k/1000L",
    cropSafetyPeriods: {}
  },
  {
    name: "Uniform",
    category: "Fungicide",
    activeIngredient: "Azoxystrobin 32.2% + Metalaxyl-M (Mefenoxam) 12.4%",
    group: "11 + 4",
    systemic: "Systemic",
    modeOfAction: "azoxystrobin is to prevent the respiration of fungi due to the disruption of electron transport chain, preventing ATP synthesis, Mefenoxam is a systemic fungicide with protective and curative properties. It is absorbed through the leaves, stems, and roots. It suppresses essential components of the disease so that the infection doesn't grow or spread.",
    manufacturer: "Syngenta, Switzerland",
    targetPests: "Root rout, Damping off, Phytophthora, Pythium, Rhizoctonia, Sclerotinia",
    safetyPeriod: 28,
    dosage: "0.2L/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "Nimbecidine",
    category: "Insecticide",
    activeIngredient: "Azadiracthin 0.03%",
    group: "limonoid group",
    systemic: "Systemic",
    modeOfAction: "acting as an Anti-feedant, Repellent, Ovi-position Deterrent, Anti-feedant, Insect Growth Regulator and Sterilant.",
    manufacturer: "T.Stanes and Company Limited, India",
    targetPests: "whiteflies, aphids, thrips, caterpillars, leafhoppers",
    safetyPeriod: 5,
    dosage: "0.003L/1L",
    cropSafetyPeriods: {}
  },
  {
    name: "Dome Soap",
    category: "Insecticide",
    activeIngredient: "potassium salt of fatty acids 5%",
    group: "biopesticide",
    systemic: "Contact",
    modeOfAction: "The lipophilic carbon chains of the fatty acids penetrate and disrupt the lipoprotein matrix of the insects cellular membranes",
    manufacturer: "Dome, Salalah, Oman",
    targetPests: "aphids, mealybugs, soft scales, armored scales, and lace bugs",
    safetyPeriod: 0,
    dosage: "0.003L/1L",
    cropSafetyPeriods: {}
  },
  {
    name: "Belthirul",
    category: "Insecticide",
    activeIngredient: "Bacillus thuringiensis (var. Kurstaki strain PB-54) 32%",
    group: "biopesticide",
    systemic: "Systemic",
    modeOfAction: "Stomach poison. Microbial disruptor of insect midgut membranes",
    manufacturer: "Probelte, S.A.U., Spain",
    targetPests: "Leaf miner, Diamondback moth, Tuta absoluta, Caterpillars",
    safetyPeriod: 0,
    dosage: "200g/200L",
    cropSafetyPeriods: {}
  },
  {
    name: "XenTari",
    category: "Insecticide",
    activeIngredient: "Bacillus thuringiensis var. aizawai",
    group: "Biopesticide",
    systemic: "Systemic",
    modeOfAction: "Stomach poison. Microbial disruptor of insect midgut membranes",
    manufacturer: "valent biosciences, Japan",
    targetPests: "caterpillar pests",
    safetyPeriod: 0,
    dosage: "3000g/1000L",
    cropSafetyPeriods: {}
  }
];

let selectedPesticide = null;

// Translation Dictionary
const lang = {
  ar: {
    appTitle: "نظام معلومات المبيدات الزراعية",
    appSubtitle: "ابحث عن المبيدات حسب الاسم، المادة الفعالة، أو المجموعة",
    searchPlaceholder: "اكتب اسم المبيد، المادة الفعالة، أو المجموعة...",
    category: "التصنيف",
    activeIngredient: "المادة الفعالة",
    group: "المجموعة",
    systemic: "تربتي",
    modeOfAction: "ميكانيكية العمل",
    manufacturer: "الشركة المصنعة",
    targetPests: "الآفات المستهدفة",
    safetyPeriod: "فترة الأمان",
    dosage: "الجرعة",
    calculatorTitle: "حاسبة الجرعة",
    waterPlaceholder: "كمية المياه (لتر)",
    dosagePlaceholder: "مثلاً: 0.25L/1000L",
    calculateBtn: "احسب",
    days: "يوم",
    resultText: "الجرعة المطلوبة: ",
    noResults: "لا توجد نتائج مطابقة",
    selectGroup: "اختر المجموعة",
    allGroups: "جميع المجموعات"
  },
  en: {
    appTitle: "Agricultural Pesticides Information System",
    appSubtitle: "Search by name, active ingredient, or group",
    searchPlaceholder: "Type name, active ingredient, or group...",
    category: "Category",
    activeIngredient: "Active Ingredient",
    group: "Group",
    systemic: "Systemic",
    modeOfAction: "Mode of Action",
    manufacturer: "Manufacturer",
    targetPests: "Target Pests",
    safetyPeriod: "Safety Period",
    dosage: "Dosage",
    calculatorTitle: "Dosage Calculator",
    waterPlaceholder: "Water Volume (L)",
    dosagePlaceholder: "e.g., 0.25L/1000L",
    calculateBtn: "Calculate",
    days: "days",
    resultText: "Required: ",
    noResults: "No matching results",
    selectGroup: "Select Group",
    allGroups: "All Groups"
  }
};

// Set Language
function setLang(l) {
  document.documentElement.setAttribute("lang", l);
  document.getElementById("html").dir = l === 'ar' ? 'rtl' : 'ltr';
  document.getElementById("html").classList.toggle("ltr", l !== 'ar');

  const t = lang[l];
  document.getElementById("appTitle").innerText = t.appTitle;
  document.getElementById("appSubtitle").innerText = t.appSubtitle;
  document.getElementById("searchInput").placeholder = t.searchPlaceholder;
  document.getElementById("lblCategory").innerText = t.category;
  document.getElementById("lblActiveIngredient").innerText = t.activeIngredient;
  document.getElementById("lblGroup").innerText = t.group;
  document.getElementById("lblSystemic").innerText = t.systemic;
  document.getElementById("lblModeOfAction").innerText = t.modeOfAction;
  document.getElementById("lblManufacturer").innerText = t.manufacturer;
  document.getElementById("lblTargetPests").innerText = t.targetPests;
  document.getElementById("lblSafetyPeriod").innerText = t.safetyPeriod;
  document.getElementById("lblDosage").innerText = t.dosage;
  document.getElementById("lblDays").innerText = t.days;
  document.getElementById("calcTitle").innerText = t.calculatorTitle;
  document.getElementById("waterVol").placeholder = t.waterPlaceholder;
  document.getElementById("dosageRatio").placeholder = t.dosagePlaceholder;
  document.getElementById("calcBtn").innerText = t.calculateBtn;
  document.getElementById("groupBtnText").innerText = t.selectGroup;
}

// Set default language
setLang('ar');

// Build Group List
function buildGroupList() {
  const groups = [...new Set(pesticides.map(p => p.group))].sort();
  const menu = document.getElementById('groupMenu');
  menu.innerHTML = '';

  // Add "All Groups"
  const allItem = document.createElement('div');
  allItem.className = 'p-2 hover:bg-gray-100 cursor-pointer border-b text-sm';
  allItem.innerText = lang[document.documentElement.lang]?.allGroups || "All Groups";
  allItem.onclick = () => {
    displayResults(pesticides);
    closeGroupMenu();
  };
  menu.appendChild(allItem);

  groups.forEach(group => {
    const item = document.createElement('div');
    item.className = 'p-2 hover:bg-gray-100 cursor-pointer border-b text-sm';
    item.innerText = group;
    item.onclick = () => {
      const filtered = pesticides.filter(p => p.group === group);
      displayResults(filtered);
      closeGroupMenu();
    };
    menu.appendChild(item);
  });
}

// Toggle Group Menu
function toggleGroupMenu() {
  const menu = document.getElementById('groupMenu');
  if (menu.classList.contains('hidden')) {
    buildGroupList();
    menu.classList.remove('hidden');
  } else {
    menu.classList.add('hidden');
  }
}

// Close Group Menu
function closeGroupMenu() {
  document.getElementById('groupMenu').classList.add('hidden');
}

// Close on click outside
document.addEventListener('click', (e) => {
  const groupBtn = document.getElementById('groupBtn');
  const groupMenu = document.getElementById('groupMenu');
  if (!groupBtn.contains(e.target) && !groupMenu.contains(e.target)) {
    groupMenu.classList.add('hidden');
  }
});

// Search Functionality
document.getElementById('searchInput').addEventListener('input', filterPesticides);

function filterPesticides(e) {
  const term = e.target.value.trim().toLowerCase();
  if (!term) return displayResults([]);

  const filtered = pesticides.filter(p =>
    p.name.toLowerCase().includes(term) ||
    p.activeIngredient.toLowerCase().includes(term) ||
    p.group.toLowerCase().includes(term)
  );

  displayResults(filtered);
}

function displayResults(list) {
  const container = document.getElementById('results');
  container.innerHTML = '';

  if (list.length === 0) {
    container.innerHTML = `<p class="col-span-full text-center text-gray-500">${lang[document.documentElement.lang]?.noResults}</p>`;
    return;
  }

  list.forEach(p => {
    const div = document.createElement('div');
    div.className = `pest-card border rounded-lg p-4 shadow-sm hover:shadow-md transition-all ${getCategoryClass(p.category)}`;
    div.innerHTML = `
      <h3 class="font-bold text-lg">${p.name}</h3>
      <p class="text-sm mt-1"><strong>${p.category}</strong></p>
      <p class="text-xs mt-1">${p.activeIngredient}</p>
      <p class="text-xs mt-1 text-gray-600">المجموعة: ${p.group}</p>
    `;
    div.onclick = () => showDetails(p);
    container.appendChild(div);
  });
}

function getCategoryClass(category) {
  if (category.includes("Insecticide")) return "category-insecticide";
  if (category.includes("Fungicide")) return "category-fungicide";
  if (category.includes("Nematicide")) return "category-nematicide";
  return "category-biopesticide";
}

// Show Details
function showDetails(p) {
  selectedPesticide = p;
  document.getElementById('detailName').textContent = p.name;
  document.getElementById('detailCategory').textContent = p.category;
  document.getElementById('detailActiveIngredient').textContent = p.activeIngredient;
  document.getElementById('detailGroup').textContent = p.group;
  document.getElementById('detailSystemic').textContent = p.systemic;
  document.getElementById('detailModeOfAction').textContent = p.modeOfAction;
  document.getElementById('detailManufacturer').textContent = p.manufacturer;
  document.getElementById('detailTargetPests').textContent = p.targetPests;
  document.getElementById('detailSafetyPeriod').textContent = p.safetyPeriod;
  document.getElementById('detailDosage').textContent = p.dosage;
  document.getElementById('details').classList.remove('hidden');
  document.getElementById('dosageRatio').value = p.dosage;
}

function closeDetails() {
  document.getElementById('details').classList.add('hidden');
}

// Dosage Calculator
function calculate() {
  const waterVolInput = document.getElementById('waterVol').value.trim();
  const dosageRatioInput = document.getElementById('dosageRatio').value.trim();

  if (!waterVolInput) {
    alert(lang[document.documentElement.lang]?.waterRequired || "Please enter water volume.");
    return;
  }

  let dosageStr;
  if (selectedPesticide && selectedPesticide.dosage) {
    dosageStr = selectedPesticide.dosage;
  } else if (dosageRatioInput) {
    dosageStr = dosageRatioInput;
  } else {
    alert(lang[document.documentElement.lang]?.enterDosage || "Please provide a dosage ratio.");
    return;
  }

  try {
    let [amtStr, waterStr] = dosageStr.split('/');
    const amtValue = parseFloat(amtStr);
    const waterValue = parseFloat(waterStr.replace(/[^\d.-]/g, ''));

    const result = (amtValue / waterValue) * waterVolInput;

    let displayResult, unit;
    if (result < 5 && result > 0) {
      displayResult = (result * 1000).toFixed(1);
      unit = 'mL';
    } else {
      displayResult = result.toFixed(2);
      unit = amtStr.includes('g') ? 'g' : 'L';
    }

    const t = lang[document.documentElement.lang];
    document.getElementById('result').innerText = `${t.resultText}${displayResult} ${unit}`;
  } catch (e) {
    alert(lang[document.documentElement.lang]?.invalidDosage || "Invalid dosage format.");
  }
}
  </script>
</body>
</html>
