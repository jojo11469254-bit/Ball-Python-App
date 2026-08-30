<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ball Python Genetics Engine</title>
    <meta name="theme-color" content="#121212">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <style>
        body { font-family: Arial, sans-serif; background-color: #121212; color: #ffffff; margin: 20px; }
        h2 { color: #4da6ff; text-align: center; margin-bottom: 5px; }
        .subtitle { text-align: center; color: #aaa; font-size: 0.9em; margin-bottom: 20px; }
        .container { max-width: 1000px; margin: 0 auto; background: #1e1e1e; padding: 20px; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.5); }
        .grid-inputs { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 15px; margin-bottom: 20px; }
        .box-container { display: flex; flex-direction: column; }
        label { font-weight: bold; margin-bottom: 5px; font-size: 0.9em; }
        .label-p1 { color: #82aaff; } .label-p2 { color: #c792ea; } .label-target { color: #ffcb6b; } .label-output { color: #c3e88d; }
        textarea { width: 100%; height: 120px; background-color: #2a2a2a; color: #ffffff; border: 1px solid #444; border-radius: 5px; padding: 10px; font-family: monospace; font-size: 0.9em; box-sizing: border-box; resize: vertical; }
        textarea:focus { border-color: #4da6ff; outline: none; }
        #outputBox { height: 380px; background-color: #0d0d0d; color: #00ffcc; font-size: 0.9em; line-height: 1.4; }
        .button-row { display: flex; gap: 10px; margin-bottom: 20px; flex-wrap: wrap; }
        button { flex: 1; min-width: 130px; padding: 12px; font-size: 0.95em; font-weight: bold; border: none; border-radius: 5px; cursor: pointer; transition: 0.2s; }
        .btn-calc { background-color: #007acc; color: white; }
        .btn-preset { background-color: #2e7d32; color: white; }
        .btn-random { background-color: #f57c00; color: white; }
        .btn-clear { background-color: #444; color: white; }
        .instructions { font-size: 0.85em; color: #aaa; margin-bottom: 15px; background: #252525; padding: 10px; border-radius: 5px; }
        .index-section { margin-top: 30px; background: #252525; padding: 15px; border-radius: 8px; border: 1px solid #333; }
        .index-title { color: #ffcb6b; font-size: 1.1em; font-weight: bold; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; }
        .search-filter { padding: 6px 12px; background: #121212; border: 1px solid #555; border-radius: 4px; color: white; font-size: 0.85em; width: 200px; }
        .morph-table-wrapper { max-height: 400px; overflow-y: auto; margin-top: 10px; border: 1px solid #333; border-radius: 4px; }
        table.morph-table { width: 100%; border-collapse: collapse; font-size: 0.85em; text-align: left; }
        table.morph-table th { background-color: #121212; color: #4da6ff; position: sticky; top: 0; padding: 10px; border-bottom: 2px solid #333; }
        table.morph-table td { padding: 8px 10px; border-bottom: 1px solid #333; }
        table.morph-table tr:hover { background-color: #2d2d2d; }
        .mm-link { display: inline-block; padding: 4px 8px; background-color: #ff6f00; color: white; text-decoration: none; border-radius: 3px; font-weight: bold; font-size: 0.8em; }
        .tag-type { display: inline-block; padding: 2px 6px; border-radius: 3px; font-size: 0.75em; font-weight: bold; }
        .tag-dominant { background-color: #1565c0; color: #90caf9; } .tag-codominant { background-color: #00838f; color: #80deea; }
        .tag-incdom { background-color: #1b5e20; color: #a5d6a7; } .tag-recessive { background-color: #b71c1c; color: #ef9a9a; }
    </style>
</head>
<body>

<div class="container">
    <h2>🐍 Ball Python Genetics Engine</h2>
    <div class="subtitle">Complete Morph Market Lexicon, Allelic Hierarchy & Integrated Visual Index</div>
    
    <div class="instructions">
        <strong>Usage:</strong><br>
        • <strong>Calculate Offspring:</strong> Fill Box 1 (Sire) and Box 2 (Dam), then click <strong>Calculate</strong>.<br>
        • <strong>Reverse Engineer Combo:</strong> Enter goal combo into <strong>Box 3</strong>, then click <strong>Calculate</strong>.<br>
    </div>

    <div class="grid-inputs">
        <div class="box-container">
            <label class="label-p1" for="p1Input">1. Parent 1 (Sire)</label>
            <textarea id="p1Input" placeholder="Pinstripe: het&#10;Cinnamon: het&#10;Piebald: het"></textarea>
        </div>

        <div class="box-container">
            <label class="label-p2" for="p2Input">2. Parent 2 (Dam)</label>
            <textarea id="p2Input" placeholder="Piebald: het"></textarea>
        </div>

        <div class="box-container">
            <label class="label-target" for="targetInput">3. Target Combo / Goal (Reverse Engineer)</label>
            <textarea id="targetInput" placeholder="Pinstripe Cinnamon Piebald&#10;Suma Pied&#10;Ivory Clown"></textarea>
        </div>
    </div>

    <div class="button-row">
        <button class="btn-calc" onclick="runGeneticsEngine()">Calculate</button>
        <button class="btn-preset" onclick="loadExampleData()">Test Demo Goal</button>
        <button class="btn-random" onclick="loadRandomDemo()">🎲 Random Demo</button>
        <button class="btn-clear" onclick="clearAll()">Clear All</button>
    </div>

    <div class="box-container">
        <label class="label-output" for="outputBox">Output Engine Results</label>
        <textarea id="outputBox" readonly placeholder="Results will be printed here..."></textarea>
    </div>

    <div class="index-section">
        <div class="index-title">
            <span>📖 Morph Database & Direct MorphMarket Examples</span>
            <input type="text" id="morphSearch" class="search-filter" placeholder="Filter morphs..." onkeyup="filterMorphIndex()">
        </div>
        <div class="morph-table-wrapper">
            <table class="morph-table" id="morphTable">
                <thead>
                    <tr>
                        <th>Morph Name</th>
                        <th>Genetics Type</th>
                        <th>Heterozygous / Single Gene Listing</th>
                        <th>Homozygous / Super Form Listing</th>
                    </tr>
                </thead>
                <tbody id="morphTableBody">
                </tbody>
            </table>
        </div>
    </div>
</div>

<script>
    const DATABASE = {
        complexes: {
            'BEL_Locus': ['Mojave', 'Lesser', 'Butter', 'Russo', 'Phantom', 'Mystic', 'Special', 'Bamboo', 'Mocha', 'Daddy Gene', 'M2', 'Lori'],
            'BlackAx_Locus': ['Cinnamon', 'Black Pastel', 'Het Red Axanthic', 'Huffman'],
            'YB_Locus': ['Yellowbelly', 'Spark', 'Specter', 'Gravel', 'Asphalt', 'Flare'],
            'Albino_Locus': ['Albino', 'Lavender Albino', 'Candy', 'Toffoo', 'Caramel Albino'],
            'Axanthic_Locus': ['Axanthic (VPI)', 'Axanthic (TSK)', 'Axanthic (MJ)', 'Dark Knight'],
            'Ultramel_Locus': ['Ultramel', 'Monarch'],
            'Scaleless_Locus': ['Scaleless Head', 'Scaleless']
        },
        dominant: ['Pinstripe', 'Spider'],
        codominant: ['Banana', 'Coral Glow', 'Champagne'],
        incDom: [
            'Pastel', 'Enchi', 'Fire', 'Vanilla', 'Yellowbelly', 'Gravel', 'Asphalt', 
            'Specter', 'Spark', 'Mojave', 'Lesser', 'Butter', 'Russo', 'Phantom', 
            'Mystic', 'Special', 'Bamboo', 'Mocha', 'Cinnamon', 'Black Pastel', 
            'Spotnose', 'Leopard', 'Black Head', 'Acid', 'Red Stripe', 'Blade', 
            'Het Red Axanthic', 'Scaleless Head', 'Mahogany', 'GHI', 'Orange Dream', 
            'Brite', 'Calico', 'Sugar', 'Trick', 'Wookie', 'Bongo', 'Cypress', 
            'Lori', 'Huffman', 'Flame'
        ],
        recessive: [
            'Piebald', 'Clown', 'Ghost', 'Hypo', 'Genetic Stripe', 'Desert Ghost', 
            'Monsoon', 'Sunset', 'Ultramel', 'Tri-stripe', 'Paint', 'Stranger',
            'Rainbow', 'Scaleless', 'Cryptic', 'Albino', 'Lavender Albino', 'Candy',
            'Toffoo', 'Caramel Albino', 'Axanthic (VPI)', 'Axanthic (TSK)', 'Axanthic (MJ)', 
            'Axanthic', 'Dark Knight', 'Confusion', 'Monarch', 'Sharps Albino', 'Patternless'
        ],
        aliases: {
            "pied": "Piebald", "dg": "Desert Ghost", "od": "Orange Dream",
            "yb": "Yellowbelly", "cg": "Banana", "coral glow": "Banana",
            "g-stripe": "Genetic Stripe", "gstripe": "Genetic Stripe",
            "phet": "het", "blkp": "Black Pastel", "cinny": "Cinnamon",
            "lav": "Lavender Albino", "lavender": "Lavender Albino",
            "tri-stripe": "Tri-stripe", "tristripe": "Tri-stripe",
            "ivory": "Super Yellowbelly", "globe": "Super Gravel", "white diamond": "Super Russo"
        },
        namedCombos: {
            "mahogany+mahogany": "Suma", "yellowbelly+yellowbelly": "Ivory",
            "gravel+gravel": "Globe", "russo+russo": "White Diamond",
            "black pastel+cinnamon": "8-Ball", "asphalt+yellowbelly": "Freeway",
            "gravel+yellowbelly": "Highway", "specter+yellowbelly": "Super Stripe",
            "spark+yellowbelly": "Puma", "lesser+mojave": "BEL",
            "butter+mojave": "BEL", "lesser+butter": "BEL",
            "mojave+phantom": "Purple Passion", "mojave+mystic": "Mystic Potion",
            "mojave+special": "Crystal", "black pastel+pastel": "Pewter",
            "cinnamon+pastel": "Pewter", "pastel+spider": "Bumblebee",
            "pinstripe+spider": "Spinner", "pastel+pinstripe": "Lemon Blast",
            "enchi+pastel": "Pastel Enchi", "fire+pastel": "Firefly",
            "fire+vanilla": "Vanilla Cream", "banana+pastel": "Banana Pastel",
            "champagne+pastel": "Pastel Champagne", "clown+leopard": "Leopard Clown",
            "acid+spotnose": "Acid Spotnose", "enchi+spotnose": "Spotnose Enchi",
            "black head+pastel": "Pastel Black Head", "cypress+pastel": "Cypress Pastel",
            "mahogany+pastel": "Suma Pastel", "lesser+pinstripe": "Kingpin",
            "spider+lesser": "Lesser Bee", "ghi+mojave": "GHI Mojave",
            "orange dream+enchi": "OD Enchi", "orange dream+fire": "Fire Dream",
            "black pastel+piebald": "Black Pastel Pied", "cinnamon+piebald": "Cinnamon Pied",
            "piebald+pinstripe": "Pinstripe Pied", "fire+yellowbelly": "Fireball",
            "bongo+pastel": "Bongo Pastel", "bongo+enchi": "Bongo Enchi",
            "spotnose+clown": "Spotnose Clown", "banana+piebald": "Banana Pied",
            "enchi+piebald": "Enchi Pied", "pastel+piebald": "Pastel Pied",
            "clown+leopard+spotnose": "Batman", "pinstripe+pastel+spider": "Spinner Blast"
        },
        morphIndexData: [
            { name: 'Pinstripe', type: 'Dominant', single: 'Pinstripe', super: 'Super Pinstripe' },
            { name: 'Spider', type: 'Dominant', single: 'Spider', super: 'Super Spider' },
            { name: 'Banana', type: 'Co-Dominant', single: 'Banana', super: 'Super Banana' },
            { name: 'Pastel', type: 'Inc-Dom', single: 'Pastel', super: 'Super Pastel' },
            { name: 'Enchi', type: 'Inc-Dom', single: 'Enchi', super: 'Super Enchi' },
            { name: 'Fire', type: 'Inc-Dom', single: 'Fire', super: 'Super Fire' },
            { name: 'Mojave', type: 'Inc-Dom', single: 'Mojave', super: 'Super Mojave' },
            { name: 'Lesser', type: 'Inc-Dom', single: 'Lesser', super: 'Super Lesser' },
            { name: 'Butter', type: 'Inc-Dom', single: 'Butter', super: 'Super Butter' },
            { name: 'Yellowbelly', type: 'Inc-Dom', single: 'Yellowbelly', super: 'Ivory' },
            { name: 'Gravel', type: 'Inc-Dom', single: 'Gravel', super: 'Super Gravel' },
            { name: 'Asphalt', type: 'Inc-Dom', single: 'Asphalt', super: 'Super Asphalt' },
            { name: 'Cinnamon', type: 'Inc-Dom', single: 'Cinnamon', super: 'Super Cinnamon' },
            { name: 'Black Pastel', type: 'Inc-Dom', single: 'Black Pastel', super: 'Super Black Pastel' },
            { name: 'Mahogany', type: 'Inc-Dom', single: 'Mahogany', super: 'Suma' },
            { name: 'Spotnose', type: 'Inc-Dom', single: 'Spotnose', super: 'Super Spotnose' },
            { name: 'Black Head', type: 'Inc-Dom', single: 'Black Head', super: 'Super Black Head' },
            { name: 'Leopard', type: 'Inc-Dom', single: 'Leopard', super: 'Super Leopard' },
            { name: 'GHI', type: 'Inc-Dom', single: 'GHI', super: 'Super GHI' },
            { name: 'Orange Dream', type: 'Inc-Dom', single: 'Orange Dream', super: 'Super Orange Dream' },
            { name: 'Piebald', type: 'Recessive', single: 'Het Piebald', super: 'Visual Piebald' },
            { name: 'Clown', type: 'Recessive', single: 'Het Clown', super: 'Visual Clown' },
            { name: 'Desert Ghost', type: 'Recessive', single: 'Het Desert Ghost', super: 'Visual Desert Ghost' },
            { name: 'Hypo', type: 'Recessive', single: 'Het Hypo', super: 'Visual Hypo' },
            { name: 'Albino', type: 'Recessive', single: 'Het Albino', super: 'Visual Albino' }
        ]
    };

    const REVERSE_NAMED_COMBOS = {};
    for (let key in DATABASE.namedCombos) {
        REVERSE_NAMED_COMBOS[DATABASE.namedCombos[key].toLowerCase()] = key.split('+');
    }

    const LOCUS_MAP = {};
    for (let locusName in DATABASE.complexes) {
        DATABASE.complexes[locusName].forEach(gene => {
            LOCUS_MAP[gene.toLowerCase()] = locusName;
        });
    }

    function getDominanceRank(traitName) {
        let clean = traitName.replace(/^(Super|Visual|100% Het|Het)\s+/i, '').trim().toLowerCase();
        if (DATABASE.dominant.some(d => d.toLowerCase() === clean)) return 1;
        if (DATABASE.codominant.some(c => c.toLowerCase() === clean)) return 2;
        if (DATABASE.incDom.some(i => i.toLowerCase() === clean)) return 3;
        if (DATABASE.recessive.some(r => r.toLowerCase() === clean)) return 4;
        return 3;
    }

    function generateMorphMarketUrl(searchTerm) {
        return `https://www.morphmarket.com/us/c/reptiles/pythons/ball-pythons?q=${encodeURIComponent(searchTerm)}`;
    }

    function populateMorphIndexTable() {
        const tbody = document.getElementById('morphTableBody');
        tbody.innerHTML = '';
        DATABASE.morphIndexData.forEach(item => {
            const tr = document.createElement('tr');
            let typeTag = '';
            if (item.type === 'Dominant') typeTag = `<span class="tag-type tag-dominant">Dominant</span>`;
            else if (item.type === 'Co-Dominant') typeTag = `<span class="tag-type tag-codominant">Co-Dominant</span>`;
            else if (item.type === 'Recessive') typeTag = `<span class="tag-type tag-recessive">Recessive</span>`;
            else typeTag = `<span class="tag-type tag-incdom">Incomplete Dom</span>`;

            tr.innerHTML = `
                <td><strong>${item.name}</strong></td>
                <td>${typeTag}</td>
                <td><a href="${generateMorphMarketUrl(item.single)}" target="_blank" class="mm-link">🔎 ${item.single}</a></td>
                <td><a href="${generateMorphMarketUrl(item.super)}" target="_blank" class="mm-link">🔎 ${item.super}</a></td>
            `;
            tbody.appendChild(tr);
        });
    }

    function filterMorphIndex() {
        const input = document.getElementById('morphSearch').value.toLowerCase();
        const rows = document.querySelectorAll('#morphTableBody tr');
        rows.forEach(row => {
            row.style.display = row.innerText.toLowerCase().includes(input) ? '' : 'none';
        });
    }

    function normalizeGeneName(geneStr) {
        let trimmed = geneStr.trim();
        let lower = trimmed.toLowerCase();
        return DATABASE.aliases[lower] ? DATABASE.aliases[lower] : trimmed;
    }

    function isRecessive(gene) {
        let normalized = normalizeGeneName(gene);
        return DATABASE.recessive.some(r => r.toLowerCase() === normalized.toLowerCase());
    }

    function capitalize(str) { return str.replace(/\b\w/g, l => l.toUpperCase()); }

    function parseInputText(text) {
        const result = {};
        if (!text.trim()) return result;
        text.split('\n').forEach(line => {
            if (line.includes(':')) {
                const parts = line.split(':');
                if (parts[0].trim() && parts[1].trim()) {
                    result[normalizeGeneName(parts[0])] = parts[1].trim().toLowerCase();
                }
            } else if (line.trim()) {
                result[normalizeGeneName(line)] = 'het';
            }
        });
        return result;
    }

    function parseLoci(parentDict) {
        const locusMap = {};
        for (let gene in parentDict) {
            const locus = LOCUS_MAP[gene.toLowerCase()] || gene;
            const status = parentDict[gene];
            if (!locusMap[locus]) locusMap[locus] = [];
            if (['visual', 'super', 'homozygous'].includes(status)) {
                locusMap[locus].push(gene, gene);
            } else {
                locusMap[locus].push(gene);
            }
        }
        const finalLoci = {};
        for (let locus in locusMap) {
            let alleles = locusMap[locus];
            while (alleles.length < 2) alleles.push('wt');
            finalLoci[locus] = alleles.slice(0, 2).sort();
        }
        return finalLoci;
    }

    function cartesianProduct(arrays) {
        return arrays.reduce((a, b) => a.flatMap(d => b.map(e => [d, e].flat())));
    }

    function resolveTwoGeneCombo(traitArray) {
        if (traitArray.length === 0) return 'Normal (Wildtype)';
        if (traitArray.length === 1) return traitArray[0];

        let remainingTraits = [...traitArray];
        let comboNames = [];

        for (let i = 0; i < remainingTraits.length; i++) {
            for (let j = i + 1; j < remainingTraits.length; j++) {
                let pairKey = [remainingTraits[i], remainingTraits[j]].map(s => s.toLowerCase()).sort().join('+');
                if (DATABASE.namedCombos[pairKey]) {
                    comboNames.push(DATABASE.namedCombos[pairKey]);
                    remainingTraits.splice(j, 1);
                    remainingTraits.splice(i, 1);
                    i--;
                    break;
                }
            }
        }

        let allResolved = [...comboNames, ...remainingTraits];
        allResolved.sort((a, b) => getDominanceRank(a) - getDominanceRank(b));
        return allResolved.join(' ');
    }

    function translatePhenotype(genotype) {
        const phenoTraits = [];
        genotype.forEach(item => {
            const [a1, a2] = item.pair;
            if (a1 === 'wt' && a2 === 'wt') return;
            if (a1 === a2) {
                let superKey = `${a1.toLowerCase()}+${a2.toLowerCase()}`;
                if (DATABASE.namedCombos[superKey]) phenoTraits.push(DATABASE.namedCombos[superKey]);
                else phenoTraits.push(isRecessive(a1) ? `Visual ${a1}` : `Super ${a1}`);
            } else if (a1 !== 'wt' && a2 !== 'wt') {
                let comboKey = [a1.toLowerCase(), a2.toLowerCase()].sort().join('+');
                if (DATABASE.namedCombos[comboKey]) phenoTraits.push(DATABASE.namedCombos[comboKey]);
                else phenoTraits.push(`${a1} ${a2}`);
            } else {
                const mut = a1 !== 'wt' ? a1 : a2;
                if (!isRecessive(mut)) phenoTraits.push(mut);
            }
        });
        return resolveTwoGeneCombo(phenoTraits);
    }

    function translateGenotype(genotype) {
        const geno = [];
        genotype.forEach(item => {
            const [a1, a2] = item.pair;
            if (a1 !== 'wt' || a2 !== 'wt') {
                if (a1 === a2) geno.push(isRecessive(a1) ? `Visual ${a1}` : `Super ${a1}`);
                else if (a1 !== 'wt' && a2 !== 'wt') geno.push(`Complex ${a1}/${a2}`);
                else geno.push(`100% Het ${a1 !== 'wt' ? a1 : a2}`);
            }
        });
        return geno.length ? geno.join(', ') : 'Normal (Wildtype)';
    }

    function calculateOffspring(p1Dict, p2Dict) {
        const p1Loci = parseLoci(p1Dict);
        const p2Loci = parseLoci(p2Dict);
        const allLoci = Array.from(new Set([...Object.keys(p1Loci), ...Object.keys(p2Loci)])).sort();

        const p1Input = allLoci.map(loc => p1Loci[loc] || ['wt', 'wt']);
        const p2Input = allLoci.map(loc => p2Loci[loc] || ['wt', 'wt']);

        let p1Gametes = p1Input.length === 1 ? p1Input[0].map(x => [x]) : cartesianProduct(p1Input);
        let p2Gametes = p2Input.length === 1 ? p2Input[0].map(x => [x]) : cartesianProduct(p2Input);

        if (!Array.isArray(p1Gametes[0])) p1Gametes = p1Gametes.map(x => [x]);
        if (!Array.isArray(p2Gametes[0])) p2Gametes = p2Gametes.map(x => [x]);

        const totalCombos = p1Gametes.length * p2Gametes.length;
        const resultsMap = {};

        p1Gametes.forEach(g1 => {
            p2Gametes.forEach(g2 => {
                const genotype = [];
                allLoci.forEach((locus, idx) => {
                    const pair = [g1[idx], g2[idx]].sort((a,b) => (a==='wt') - (b==='wt') || a.localeCompare(b));
                    genotype.push({ locus: locus, pair: pair });
                });
                const key = JSON.stringify(genotype);
                resultsMap[key] = (resultsMap[key] || 0) + 1;
            });
        });

        let outputText = `=== POSSIBLE OFFSPRING COMBINATIONS ===\nTotal Hatchling Possibilities: ${totalCombos}\n\n`;
        let countIndex = 1;
        for (let key in resultsMap) {
            const geno = JSON.parse(key);
            const count = resultsMap[key];
            const percent = ((count / totalCombos) * 100).toFixed(2);
            outputText += `#${countIndex} [Odds: ${count}/${totalCombos} | ${percent}%]\n`;
            outputText += `   Visual Phenotype : ${translatePhenotype(geno)}\n`;
            outputText += `   Genotype Breakdown: ${translateGenotype(geno)}\n\n`;
            countIndex++;
        }
        return outputText;
    }

    function runGeneticsEngine() {
        const p1Text = document.getElementById('p1Input').value;
        const p2Text = document.getElementById('p2Input').value;
        const outputBox = document.getElementById('outputBox');
        if (p1Text.trim() || p2Text.trim()) {
            outputBox.value = calculateOffspring(parseInputText(p1Text), parseInputText(p2Text));
        } else {
            outputBox.value = "Please fill in Box 1 and Box 2 to calculate offspring.";
        }
    }

    function loadExampleData() {
        document.getElementById('p1Input').value = "Pinstripe: het\nCinnamon: het\nPiebald: het";
        document.getElementById('p2Input').value = "Piebald: het";
        runGeneticsEngine();
    }

    function loadRandomDemo() {
        document.getElementById('p1Input').value = "Pastel: het\nYellowbelly: het";
        document.getElementById('p2Input').value = "Pastel: het\nPiebald: het";
        runGeneticsEngine();
    }

    function clearAll() {
        document.getElementById('p1Input').value = '';
        document.getElementById('p2Input').value = '';
        document.getElementById('targetInput').value = '';
        document.getElementById('outputBox').value = '';
    }

    window.onload = populateMorphIndexTable;
</script>
</body>
</html>


