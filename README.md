<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>قائمة المهام - إدارة المحاضرات</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        
        .container {
            width: 100%;
            max-width: 800px;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }
        
        header {
            background: #4a6fc7;
            color: white;
            padding: 20px;
            text-align: center;
        }
        
        h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }
        
        .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }
        
        .stats {
            display: flex;
            gap: 15px;
        }
        
        .stat-box {
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 12px;
            border-radius: 8px;
            text-align: center;
        }
        
        .stat-number {
            display: block;
            font-size: 18px;
            font-weight: bold;
        }
        
        .stat-label {
            font-size: 12px;
        }
        
        .clear-btn {
            background: rgba(255, 255, 255, 0.2);
            color: white;
            border: none;
            padding: 8px 15px;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .clear-btn:hover {
            background: rgba(255, 255, 255, 0.3);
        }
        
        .input-section {
            display: flex;
            padding: 20px;
            gap: 10px;
            border-bottom: 1px solid #eee;
        }
        
        input, select, button {
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
        }
        
        input {
            flex-grow: 1;
        }
        
        button {
            background-color: #4a6fc7;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            gap: 5px;
        }
        
        button:hover {
            background-color: #3b5aa6;
        }
        
        .filters {
            display: flex;
            padding: 0 20px;
            gap: 10px;
            margin-bottom: 20px;
        }
        
        .filter-btn {
            background-color: #f0f0f0;
            color: #555;
        }
        
        .filter-btn.active {
            background-color: #4a6fc7;
            color: white;
        }
        
        .lecture-list {
            list-style-type: none;
            padding: 0 20px 20px;
            max-height: 400px;
            overflow-y: auto;
        }
        
        .lecture-item {
            display: flex;
            align-items: center;
            padding: 15px;
            margin-bottom: 10px;
            background-color: #f9f9f9;
            border-radius: 10px;
            border-left: 5px solid #4a6fc7;
            transition: transform 0.2s;
        }
        
        .lecture-item:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        
        .lecture-item.completed {
            border-left-color: #2ecc71;
            opacity: 0.8;
        }
        
        .lecture-item.completed .lecture-title {
            text-decoration: line-through;
            color: #777;
        }
        
        .lecture-info {
            flex-grow: 1;
        }
        
        .lecture-title {
            font-weight: bold;
            margin-bottom: 5px;
        }
        
        .lecture-details {
            font-size: 0.9em;
            color: #777;
            display: flex;
            gap: 15px;
        }
        
        .actions {
            display: flex;
            gap: 5px;
        }
        
        .toggle-btn, .delete-btn {
            padding: 8px 12px;
            font-size: 14px;
        }
        
        .delete-btn {
            background-color: #e74c3c;
        }
        
        .delete-btn:hover {
            background-color: #c0392b;
        }
        
        .empty-state {
            text-align: center;
            padding: 30px;
            color: #777;
        }
        
        .empty-state i {
            font-size: 50px;
            margin-bottom: 15px;
            color: #ddd;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .lecture-item {
            animation: fadeIn 0.3s ease;
        }
        
        @media (max-width: 600px) {
            .input-section {
                flex-direction: column;
            }
            
            .header-controls {
                flex-direction: column;
                gap: 15px;
            }
            
            .stats {
                width: 100%;
                justify-content: space-around;
            }
            
            .filters {
                flex-wrap: wrap;
            }
            
            .filter-btn {
                flex-grow: 1;
                text-align: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>إدارة المحاضرات الدراسية</h1>
            <div class="header-controls">
                <div class="stats">
                    <div class="stat-box">
                        <span class="stat-number" id="totalLectures">0</span>
                        <span class="stat-label">المحاضرات</span>
                    </div>
                    <div class="stat-box">
                        <span class="stat-number" id="completedLectures">0</span>
                        <span class="stat-label">مكتملة</span>
                    </div>
                </div>
                <button id="clearAllBtn" class="clear-btn">مسح الكل</button>
            </div>
        </header>
        
        <div class="input-section">
            <input type="text" id="lectureInput" placeholder="أدخل اسم المحاضرة">
            <select id="subjectSelect">
                <option value="عام">عام</option>
                <option value="رياضيات">رياضيات</option>
                <option value="فيزياء">فيزياء</option>
                <option value="كيمياء">كيمياء</option>
                <option value="أحياء">أحياء</option>
                <option value="لغة عربية">لغة عربية</option>
                <option value="لغة إنجليزية">لغة إنجليزية</option>
            </select>
            <button id="addBtn"><i class="fas fa-plus"></i> إضافة</button>
        </div>
        
        <div class="filters">
            <button class="filter-btn active" data-filter="all">جميع المحاضرات</button>
            <button class="filter-btn" data-filter="pending">المعلقة</button>
            <button class="filter-btn" data-filter="completed">المكتملة</button>
        </div>
        
        <ul id="lectureList" class="lecture-list">
            <!-- المحاضرات ستضاف هنا ديناميكيًا -->
        </ul>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const lectureInput = document.getElementById('lectureInput');
            const subjectSelect = document.getElementById('subjectSelect');
            const addBtn = document.getElementById('addBtn');
            const lectureList = document.getElementById('lectureList');
            const filterBtns = document.querySelectorAll('.filter-btn');
            const clearAllBtn = document.getElementById('clearAllBtn');
            const totalLecturesEl = document.getElementById('totalLectures');
            const completedLecturesEl = document.getElementById('completedLectures');
            
            let lectures = JSON.parse(localStorage.getItem('lectures')) || [];
            let currentFilter = 'all';
            
            // تهيئة التطبيق
            function init() {
                renderLectures();
                updateStats();
                
                // تحميل المحاضرات الافتراضية إذا لم يكن هناك محاضرات
                if (lectures.length === 0) {
                    addDefaultLectures();
                }
            }
            
            // إضافة بعض المحاضرات الافتراضية
            function addDefaultLectures() {
                const defaultLectures = [
                    { title: 'مراجعة الكيمياء العضوية', subject: 'كيمياء', completed: false, date: new Date() },
                    { title: 'حل مسائل التفاضل', subject: 'رياضيات', completed: true, date: new Date() },
                    { title: 'قراءة النص الأدبي', subject: 'لغة عربية', completed: false, date: new Date() }
                ];
                
                lectures = [...defaultLectures, ...lectures];
                localStorage.setItem('lectures', JSON.stringify(lectures));
                renderLectures();
                updateStats();
            }
            
            // عرض المحاضرات
            function renderLectures() {
                lectureList.innerHTML = '';
                
                const filteredLectures = lectures.filter(lecture => {
                    if (currentFilter === 'all') return true;
                    if (currentFilter === 'completed') return lecture.completed;
                    if (currentFilter === 'pending') return !lecture.completed;
                    return true;
                });
                
                if (filteredLectures.length === 0) {
                    lectureList.innerHTML = `
                        <div class="empty-state">
                            <i class="fas fa-clipboard-list"></i>
                            <p>لا توجد محاضرات لعرضها</p>
                        </div>
                    `;
                    return;
                }
                
                filteredLectures.forEach((lecture, index) => {
                    const li = document.createElement('li');
                    li.className = `lecture-item ${lecture.completed ? 'completed' : ''}`;
                    
                    // تنسيق التاريخ
                    const date = new Date(lecture.date);
                    const formattedDate = date.toLocaleDateString('ar-EG', {
                        weekday: 'long',
                        year: 'numeric',
                        month: 'long',
                        day: 'numeric'
                    });
                    
                    li.innerHTML = `
                        <div class="lecture-info">
                            <div class="lecture-title">${lecture.title}</div>
                            <div class="lecture-details">
                                <span>المادة: ${lecture.subject}</span>
                                <span>التاريخ: ${formattedDate}</span>
                            </div>
                        </div>
                        <div class="actions">
                            <button class="toggle-btn">${lecture.completed ? '<i class="fas fa-undo"></i> إلغاء' : '<i class="fas fa-check"></i> إكمال'}</button>
                            <button class="delete-btn"><i class="fas fa-trash"></i> حذف</button>
                        </div>
                    `;
                    
                    lectureList.appendChild(li);
                    
                    // إضافة event listeners للأزرار
                    li.querySelector('.toggle-btn').addEventListener('click', () => {
                        toggleLecture(index);
                    });
                    
                    li.querySelector('.delete-btn').addEventListener('click', () => {
                        deleteLecture(index);
                    });
                });
                
                // حفظ في localStorage
                localStorage.setItem('lectures', JSON.stringify(lectures));
            }
            
            // تحديث الإحصائيات
            function updateStats() {
                totalLecturesEl.textContent = lectures.length;
                const completedCount = lectures.filter(lecture => lecture.completed).length;
                completedLecturesEl.textContent = completedCount;
            }
            
            // إضافة محاضرة جديدة
            function addLecture() {
                const title = lectureInput.value.trim();
                const subject = subjectSelect.value;
                
                if (title) {
                    lectures.push({
                        title,
                        subject,
                        completed: false,
                        date: new Date()
                    });
                    
                    lectureInput.value = '';
                    renderLectures();
                    updateStats();
                    
                    // إظهار رسالة نجاح
                    showNotification('تم إضافة المحاضرة بنجاح!');
                } else {
                    showNotification('يرجى إدخال اسم المحاضرة', 'error');
                }
            }
            
            // تبديل حالة المحاضرة (مكتملة/غير مكتملة)
            function toggleLecture(index) {
                lectures[index].completed = !lectures[index].completed;
                renderLectures();
                updateStats();
                
                // إظهار رسالة نجاح
                const message = lectures[index].completed ? 
                    'تهانينا! لقد أكملت هذه المحاضرة' : 
                    'تم إعادة فتح المحاضرة';
                showNotification(message);
            }
            
            // حذف محاضرة
            function deleteLecture(index) {
                if (confirm('هل أنت متأكد من أنك تريد حذف هذه المحاضرة؟')) {
                    lectures.splice(index, 1);
                    renderLectures();
                    updateStats();
                    showNotification('تم حذف المحاضرة بنجاح');
                }
            }
            
            // مسح جميع المحاضرات
            function clearAllLectures() {
                if (lectures.length === 0) {
                    showNotification('لا توجد محاضرات لمسحها', 'error');
                    return;
                }
                
                if (confirm('هل أنت متأكد من أنك تريد مسح جميع المحاضرات؟ لا يمكن التراجع عن هذا الإجراء.')) {
                    lectures = [];
                    localStorage.setItem('lectures', JSON.stringify(lectures));
                    renderLectures();
                    updateStats();
                    showNotification('تم مسح جميع المحاضرات بنجاح');
                }
            }
            
            // إظهار إشعار
            function showNotification(message, type = 'success') {
                // إنصراف إذا كان هناك إشعار موجود بالفعل
                if (document.querySelector('.notification')) return;
                
                const notification = document.createElement('div');
                notification.className = `notification ${type}`;
                notification.textContent = message;
                
                // إضافة الأنماط
                notification.style.position = 'fixed';
                notification.style.bottom = '20px';
                notification.style.left = '50%';
                notification.style.transform = 'translateX(-50%)';
                notification.style.padding = '12px 24px';
                notification.style.background = type === 'success' ? '#2ecc71' : '#e74c3c';
                notification.style.color = 'white';
                notification.style.borderRadius = '8px';
                notification.style.boxShadow = '0 4px 12px rgba(0,0,0,0.15)';
                notification.style.zIndex = '1000';
                notification.style.opacity = '0';
                notification.style.transition = 'opacity 0.3s';
                
                document.body.appendChild(notification);
                
                // إظهار الإشعار
                setTimeout(() => {
                    notification.style.opacity = '1';
                }, 10);
                
                // إخفاء الإشعار بعد 3 ثوان
                setTimeout(() => {
                    notification.style.opacity = '0';
                    setTimeout(() => {
                        notification.remove();
                    }, 300);
                }, 3000);
            }
            
            // event listeners
            addBtn.addEventListener('click', addLecture);
            
            lectureInput.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    addLecture();
                }
            });
            
            filterBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    filterBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    currentFilter = btn.getAttribute('data-filter');
                    renderLectures();
                });
            });
            
            clearAllBtn.addEventListener('click', clearAllLectures);
            
            // التهيئة الأولية
            init();
        });
    </script>
</body>
</html>
