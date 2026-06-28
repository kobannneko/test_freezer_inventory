import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-app.js";
import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-auth.js";
import { getFirestore, collection, doc, addDoc, updateDoc, deleteDoc, onSnapshot, serverTimestamp, writeBatch } from "https://www.gstatic.com/firebasejs/10.12.5/firebase-firestore.js";

const APP_VERSION="3.0";
const LOCAL_STORAGE_KEY="freezer_inventory_app_data";
const MIGRATION_DISMISSED_KEY="furikore_migration_dismissed_v2";
const FIRESTORE_PATH=["households","default","areas","freezer","items"];
const CATEGORIES=["鶏肉","豚肉","牛肉","加工肉","魚介類","野菜","主食","冷凍食品","アイス","お菓子","調味料","作り置き","その他"];

const firebaseConfig={
  apiKey:"AIzaSyCo76wuPHhxCd3NH_qGoQxplxhVLNhrYsQ",
  authDomain:"furikore-395e8.firebaseapp.com",
  projectId:"furikore-395e8",
  storageBucket:"furikore-395e8.firebasestorage.app",
  messagingSenderId:"608348115093",
  appId:"1:608348115093:web:84623fa7dd36cd5e94cb9c"
};

const app=initializeApp(firebaseConfig);
const auth=getAuth(app);
const db=getFirestore(app);
const provider=new GoogleAuthProvider();
const itemsRef=collection(db,...FIRESTORE_PATH);

let items=[];
let unsubscribeItems=null;
let mode="display";

const $=(id)=>document.getElementById(id);
const els={
  authStatus:$("authStatus"),loginBtn:$("loginBtn"),logoutBtn:$("logoutBtn"),mainApp:$("mainApp"),
  itemCount:$("itemCount"),syncStatus:$("syncStatus"),migrationCard:$("migrationCard"),migrateBtn:$("migrateBtn"),dismissMigrationBtn:$("dismissMigrationBtn"),
  displayTab:$("displayTab"),inputTab:$("inputTab"),displayPanel:$("displayPanel"),inputPanel:$("inputPanel"),
  displaySearch:$("displaySearch"),displayCategory:$("displayCategory"),displaySort:$("displaySort"),displayList:$("displayList"),
  form:$("itemForm"),formTitle:$("formTitle"),editingId:$("editingId"),name:$("nameInput"),category:$("categoryInput"),
  quantity:$("quantityInput"),unit:$("unitInput"),frozenDate:$("frozenDateInput"),location:$("locationInput"),memo:$("memoInput"),cancelEdit:$("cancelEditBtn"),
  recentList:$("recentList"),exportBtn:$("exportBtn"),importInput:$("importInput"),template:$("displayItemTemplate")
};

function todayIso(){return new Date().toISOString().slice(0,10)}
function daysSince(s){if(!s)return null;const d=new Date(`${s}T00:00:00`);if(Number.isNaN(d.getTime()))return null;const n=new Date();const t=new Date(n.getFullYear(),n.getMonth(),n.getDate());return Math.floor((t-d)/86400000)}
function roundQty(v){return Math.round((Number(v)||0)*4)/4}
function formatQty(v){const n=roundQty(v);return Number.isInteger(n)?String(n):String(n).replace(/\.?0+$/,"")}
function normalizeCategory(c){const map={肉:"鶏肉",魚:"魚介類"};return map[c]||(CATEGORIES.includes(c)?c:"その他")}
function normalizeItem(raw={}){return{name:String(raw.name||raw.itemName||raw.title||"").trim(),category:normalizeCategory(raw.category||"その他"),quantity:roundQty(raw.quantity??raw.amount??raw.count??1),unit:raw.unit||"個",frozenDate:raw.frozenDate||raw.date||raw.freezingDate||todayIso(),location:raw.location||raw.place||"未設定",memo:raw.memo||raw.note||"",schemaVersion:2}}
function ageClass(age){if(age===null)return"fresh";if(age>=61)return"very-old";if(age>=31)return"old";if(age>=15)return"watch";return"fresh"}
function ageLabel(age){return age===null?"日付未設定":`冷凍${age}日`}
function escapeHtml(str){return String(str).replace(/[&<>"']/g,s=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"}[s]))}

function getDisplayItems(){
  const q=els.displaySearch.value.trim().toLowerCase();
  const cat=els.displayCategory.value;
  const sort=els.displaySort.value;
  return items.filter(item=>{
    const hay=`${item.name} ${item.category} ${item.location} ${item.memo}`.toLowerCase();
    return (!q||hay.includes(q))&&(cat==="all"||item.category===cat);
  }).sort((a,b)=>{
    const da=daysSince(a.frozenDate)??-1, db=daysSince(b.frozenDate)??-1;
    if(sort==="ageAsc"){if(da!==db)return da-db}
    else if(sort==="category")return `${a.category}${a.name}`.localeCompare(`${b.category}${b.name}`,"ja");
    else if(sort==="name")return a.name.localeCompare(b.name,"ja");
    else if(sort==="quantityDesc"){if(Number(b.quantity)!==Number(a.quantity))return Number(b.quantity)-Number(a.quantity)}
    else if(sort==="quantityAsc"){if(Number(a.quantity)!==Number(b.quantity))return Number(a.quantity)-Number(b.quantity)}
    else {if(db!==da)return db-da}
    return a.name.localeCompare(b.name,"ja");
  });
}

function render(){
  els.itemCount.textContent=`${items.length}件`;
  els.displayPanel.classList.toggle("hidden",mode!=="display");
  els.inputPanel.classList.toggle("hidden",mode!=="input");
  els.displayTab.classList.toggle("active",mode==="display");
  els.inputTab.classList.toggle("active",mode==="input");
  renderDisplayList();
  renderRecent();
}

function renderDisplayList(){
  const list=getDisplayItems();
  els.displayList.innerHTML="";
  if(!list.length){els.displayList.innerHTML='<div class="card empty">在庫がありません。入力タブから登録しましょう。</div>';return}
  for(const item of list){
    const node=els.template.content.firstElementChild.cloneNode(true);
    const age=daysSince(item.frozenDate), cls=ageClass(age);
    node.classList.add(`age-${cls}`);
    node.querySelector(".item-title").textContent=item.name||"名称未設定";
    node.querySelector(".item-meta").textContent=`${item.category}・${item.location}・${item.frozenDate||"日付未設定"}`;
    node.querySelector(".qty-display").textContent=`${formatQty(item.quantity)}${item.unit}`;
    const ageBadge=node.querySelector(".age-badge"); ageBadge.textContent=ageLabel(age); if(cls!=="fresh")ageBadge.classList.add(cls);
    const memo=node.querySelector(".memo-line"); memo.textContent=item.memo?`メモ：${item.memo}`:""; memo.classList.toggle("hidden",!item.memo);
    node.querySelector(".dec-one").addEventListener("click",()=>changeQty(item,-1));
    node.querySelector(".dec-quarter").addEventListener("click",()=>changeQty(item,-0.25));
    node.querySelector(".inc-quarter").addEventListener("click",()=>changeQty(item,0.25));
    node.querySelector(".inc-one").addEventListener("click",()=>changeQty(item,1));
    node.querySelector(".menu-btn").addEventListener("click",()=>openItemMenu(item));
    els.displayList.appendChild(node);
  }
}

function getRecentTemplates(){
  const map=new Map();
  const sorted=[...items].filter(i=>i.name).sort((a,b)=>(b.createdAt?.seconds||b.updatedAt?.seconds||0)-(a.createdAt?.seconds||a.updatedAt?.seconds||0));
  for(const i of sorted){const key=`${i.name}|${i.category}|${i.unit}`;if(!map.has(key))map.set(key,i);if(map.size>=6)break}
  return[...map.values()];
}
function renderRecent(){
  els.recentList.innerHTML="";
  const recent=getRecentTemplates();
  if(!recent.length){els.recentList.innerHTML='<p class="muted">登録後によく使う候補が表示されます。</p>';return}
  recent.forEach(item=>{
    const b=document.createElement("button");b.type="button";b.className="recent-chip";b.textContent=item.name;
    b.addEventListener("click",()=>{resetForm();els.name.value=item.name;els.category.value=item.category;els.quantity.value="1";els.unit.value=item.unit||"個";els.location.value=item.location||"未設定";els.memo.value=item.memo||"";els.frozenDate.value=todayIso();window.scrollTo({top:els.form.offsetTop-10,behavior:"smooth"})});
    els.recentList.appendChild(b);
  });
}

async function changeQty(item,delta){
  const next=roundQty(Number(item.quantity||0)+delta);
  if(next<=0){
    const remove=confirm(`${item.name}が0になりました。\n在庫から削除しますか？\n\nOK：削除\nキャンセル：0のまま残す`);
    if(remove) await deleteDoc(doc(db,...FIRESTORE_PATH,item.id));
    else await updateDoc(doc(db,...FIRESTORE_PATH,item.id),{quantity:0,updatedAt:serverTimestamp()});
    return;
  }
  await updateDoc(doc(db,...FIRESTORE_PATH,item.id),{quantity:next,updatedAt:serverTimestamp()});
}
function openItemMenu(item){
  const action=prompt(`${item.name}\n\n1: 編集\n2: 削除\n\n数字を入力してください`);
  if(action==="1")startEdit(item);
  if(action==="2")finishItem(item);
}
function startEdit(item){
  mode="input";render();
  els.editingId.value=item.id;els.formTitle.textContent="在庫を編集";
  els.name.value=item.name;els.category.value=item.category;els.quantity.value=formatQty(item.quantity);
  els.unit.value=item.unit;els.frozenDate.value=item.frozenDate||todayIso();els.location.value=item.location||"未設定";els.memo.value=item.memo||"";
  els.cancelEdit.classList.remove("hidden");
  window.scrollTo({top:document.getElementById("formCard").offsetTop-10,behavior:"smooth"});
}
async function finishItem(item){if(confirm(`${item.name}を削除しますか？`))await deleteDoc(doc(db,...FIRESTORE_PATH,item.id))}
function resetForm(){els.form.reset();els.editingId.value="";els.formTitle.textContent="在庫を追加";els.quantity.value="1";els.frozenDate.value=todayIso();els.category.value="鶏肉";els.cancelEdit.classList.add("hidden")}

async function saveForm(e){
  e.preventDefault();
  const payload=normalizeItem({name:els.name.value,category:els.category.value,quantity:els.quantity.value,unit:els.unit.value,frozenDate:els.frozenDate.value,location:els.location.value,memo:els.memo.value});
  if(!payload.name)return;
  const id=els.editingId.value;
  if(id) await updateDoc(doc(db,...FIRESTORE_PATH,id),{...payload,updatedAt:serverTimestamp()});
  else await addDoc(itemsRef,{...payload,createdAt:serverTimestamp(),updatedAt:serverTimestamp()});
  resetForm();
  mode="input";
  render();
}

function readLocalData(){try{const raw=localStorage.getItem(LOCAL_STORAGE_KEY);if(!raw)return[];const p=JSON.parse(raw);if(Array.isArray(p))return p.map(normalizeItem).filter(x=>x.name);if(Array.isArray(p.items))return p.items.map(normalizeItem).filter(x=>x.name);return[]}catch{return[]}}
function showMigrationIfNeeded(){const local=readLocalData();const dismissed=localStorage.getItem(MIGRATION_DISMISSED_KEY)==="1";els.migrationCard.classList.toggle("hidden",!(local.length&&!dismissed))}
async function migrateLocalData(){const local=readLocalData();if(!local.length)return;if(!confirm(`${local.length}件をFirebaseへコピーします。実行しますか？`))return;const batch=writeBatch(db);for(const item of local){const ref=doc(itemsRef);batch.set(ref,{...item,migratedFromLocal:true,createdAt:serverTimestamp(),updatedAt:serverTimestamp()})}await batch.commit();localStorage.setItem(MIGRATION_DISMISSED_KEY,"1");els.migrationCard.classList.add("hidden")}
function exportJson(){const data={app:"furikore",appVersion:APP_VERSION,exportedAt:new Date().toISOString(),schemaVersion:2,items:items.map(({id,...rest})=>rest)};const blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"});const url=URL.createObjectURL(blob);const a=document.createElement("a");a.href=url;a.download=`furikore_backup_${todayIso()}.json`;a.click();URL.revokeObjectURL(url)}
async function importJson(file){if(!file)return;const text=await file.text();const parsed=JSON.parse(text);const arr=Array.isArray(parsed)?parsed:parsed.items;if(!Array.isArray(arr)){alert("読み込める在庫データが見つかりませんでした。");return}if(!confirm(`${arr.length}件をFirebaseへ追加します。よろしいですか？`))return;const batch=writeBatch(db);arr.map(normalizeItem).filter(x=>x.name).forEach(item=>{const ref=doc(itemsRef);batch.set(ref,{...item,importedAt:serverTimestamp(),createdAt:serverTimestamp(),updatedAt:serverTimestamp()})});await batch.commit();els.importInput.value=""}

function subscribeItems(){
  if(unsubscribeItems)unsubscribeItems();
  els.syncStatus.textContent="同期中";
  unsubscribeItems=onSnapshot(itemsRef,snap=>{
    items=snap.docs.map(d=>{const raw=d.data();return{id:d.id,...normalizeItem(raw),createdAt:raw.createdAt,updatedAt:raw.updatedAt}});
    els.syncStatus.textContent="同期済み";render();
  },err=>{console.error(err);els.syncStatus.textContent="同期エラー";alert("Firestoreの読み込みに失敗しました。")});
}
function setupEvents(){
  els.loginBtn.addEventListener("click",()=>signInWithPopup(auth,provider));
  els.logoutBtn.addEventListener("click",()=>signOut(auth));
  els.displayTab.addEventListener("click",()=>{mode="display";render()});
  els.inputTab.addEventListener("click",()=>{mode="input";render()});
  els.displaySearch.addEventListener("input",render);
  els.displayCategory.addEventListener("change",render);
  els.displaySort.addEventListener("change",render);
  els.form.addEventListener("submit",saveForm);
  els.cancelEdit.addEventListener("click",resetForm);
  els.migrateBtn.addEventListener("click",migrateLocalData);
  els.dismissMigrationBtn.addEventListener("click",()=>{localStorage.setItem(MIGRATION_DISMISSED_KEY,"1");els.migrationCard.classList.add("hidden")});
  els.exportBtn.addEventListener("click",exportJson);
  els.importInput.addEventListener("change",e=>importJson(e.target.files?.[0]));
}
onAuthStateChanged(auth,user=>{
  if(user){
    els.authStatus.textContent=`${user.email} でログイン中`;els.loginBtn.classList.add("hidden");els.logoutBtn.classList.remove("hidden");els.mainApp.classList.remove("hidden");
    resetForm();showMigrationIfNeeded();subscribeItems();
  }else{
    els.authStatus.textContent="Googleアカウントでログインすると、夫婦で同じ在庫を共有できます。";els.loginBtn.classList.remove("hidden");els.logoutBtn.classList.add("hidden");els.mainApp.classList.add("hidden");
    if(unsubscribeItems)unsubscribeItems();items=[];
  }
});
setupEvents();
