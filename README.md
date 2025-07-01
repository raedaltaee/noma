<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>حقيبة تدريب NOMA التفاعلية</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    
    <!-- Chosen Palette: Warm Neutrals with Teal and Indigo Accents -->
    <!-- Application Structure Plan: تم تصميم التطبيق كهيكل دورة تدريبية تفاعلية من صفحة واحدة. يحتوي على شريط تنقل جانبي (sidebar) يسمح بالتبديل بين المحاضرات الخمس. كل محاضرة هي قسم مستقل يتضمن المحتوى العلمي، يليه قسم للأنشطة التفاعلية والمهام. تم اختيار هذا الهيكل لمحاكاة تجربة الدورة التدريبية الفعلية، حيث يمكن للمستخدمين التقدم عبر المحاضرات بشكل منظم، والتفاعل مع المحتوى مباشرة بعد تعلمه. يضمن هذا التصميم أن تكون الأنشطة والمهام جزءًا لا يتجزأ من تجربة التعلم، مما يعزز الفهم العملي. -->
    <!-- Visualization & Content Choices: 
        - Report Info: Lecture Content. Goal: Inform. Viz: Text blocks, lists, structured HTML. Justification: Presenting the detailed lecture content as outlined in the source report.
        - Report Info: OMA vs NOMA comparison. Goal: Compare. Viz: Interactive Comparison Table & Radar Chart. Interaction: Toggle details, chart update. Justification: A table allows direct comparison of features, while the radar chart provides a visual summary of performance metrics.
        - Report Info: SIC Process. Goal: Organize/Inform. Viz: Step-by-Step Interactive Diagram. Interaction: Next/Previous step buttons. Justification: Visualizing the sequential process of SIC step-by-step makes this complex algorithm much easier to understand than descriptive text alone.
        - Report Info: Challenges & Mitigations. Goal: Organize. Viz: Accordion component. Justification: An accordion presents a clean overview of challenges, allowing the user to expand and explore details on demand without overwhelming the initial view.
        - Report Info: Performance Metrics. Goal: Inform. Viz: Bar Chart (Chart.js). Justification: Clearly shows quantitative improvements.
        - Report Info: Applications. Goal: Inform. Viz: Icon-based Card Grid. Justification: Cards provide a scannable, visually appealing summary of various technologies and use-cases.
        - Report Info: Interactive Activities/Tasks. Goal: Engage/Assess. Viz: Quizzes (MCQ), Fill-in-the-blank, Open-ended text areas. Interaction: Submit, check answers, show feedback. Justification: Directly implements the suggested activities from the source report to provide a hands-on learning experience.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->

    <style>
        body {
            font-family: 'Cairo', sans-serif;
            scroll-behavior: smooth;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
        .active-lecture-link {
            background-color: #e0f2f2; /* teal-50 */
            color: #0d9488; /* teal-600 */
            font-weight: 700;
            border-right: 4px solid #0d9488;
        }
        .lecture-content {
            display: none;
        }
        .lecture-content.active {
            display: block;
        }
        .quiz-option {
            cursor: pointer;
            transition: background-color 0.2s;
        }
        .quiz-option:hover {
            background-color: #f0fdf4; /* emerald-50 */
        }
        .quiz-option.selected {
            background-color: #d1fae5; /* emerald-100 */
            border-color: #34d399; /* emerald-400 */
        }
        .quiz-option.correct {
            background-color: #d1fae5; /* emerald-100 */
            border-color: #059669; /* emerald-600 */
        }
        .quiz-option.incorrect {
            background-color: #fee2e2; /* red-100 */
            border-color: #ef4444; /* red-500 */
        }
        .quiz-feedback {
            margin-top: 10px;
            font-weight: bold;
        }
        .accordion-item {
            border-radius: 0.5rem;
            overflow: hidden;
        }
        .accordion-header {
            background-color: #f8fafc; /* slate-50 */
            padding: 1rem;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid #e2e8f0; /* slate-200 */
        }
        .accordion-header:hover {
            background-color: #f1f5f9; /* slate-100 */
        }
        .accordion-content {
            padding: 1rem;
            background-color: #ffffff;
            border-top: 1px solid #e2e8f0;
        }
        .accordion-icon {
            transition: transform 0.3s ease-in-out;
        }
        .accordion-header[aria-expanded="true"] .accordion-icon {
            transform: rotate(45deg);
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-800">

    <div class="flex flex-col md:flex-row min-h-screen">
        <!-- Sidebar Navigation -->
        <aside class="w-full md:w-64 bg-white shadow-lg p-6 md:p-8 flex-shrink-0">
            <h2 class="text-2xl font-bold text-teal-700 mb-6 border-b pb-4">حقيبة تدريب NOMA</h2>
            <nav>
                <ul class="space-y-2">
                    <li><a href="#" data-lecture="lecture1" class="lecture-link block p-3 rounded-lg text-slate-700 hover:bg-slate-100 transition-colors active-lecture-link">المحاضرة 1: أساسيات NOMA والدوافع</a></li>
                    <li><a href="#" data-lecture="lecture2" class="lecture-link block p-3 rounded-lg text-slate-700 hover:bg-slate-100 transition-colors">المحاضرة 2: PD-NOMA و SIC</a></li>
                    <li><a href="#" data-lecture="lecture3" class="lecture-link block p-3 rounded-lg text-slate-700 hover:bg-slate-100 transition-colors">المحاضرة 3: CD-NOMA، الأداء، والتحديات</a></li>
                    <li><a href="#" data-lecture="lecture4" class="lecture-link block p-3 rounded-lg text-slate-700 hover:bg-slate-100 transition-colors">المحاضرة 4: تكاملات NOMA المتقدمة</a></li>
                    <li><a href="#" data-lecture="lecture5" class="lecture-link block p-3 rounded-lg text-slate-700 hover:bg-slate-100 transition-colors">المحاضرة 5: التطبيقات والآفاق المستقبلية</a></li>
                </ul>
            </nav>
        </aside>

        <!-- Main Content Area -->
        <main class="flex-1 p-6 md:p-10">

            <!-- Lecture 1 Content -->
            <section id="lecture1" class="lecture-content active bg-white p-8 rounded-lg shadow-xl mb-10">
                <h3 class="text-3xl font-bold text-slate-900 mb-6">المحاضرة 1: أساسيات NOMA والدوافع</h3>
                <p class="text-lg text-slate-600 mb-6">في هذه المحاضرة، سنستكشف تطور تقنيات الوصول المتعدد، ونفهم لماذا أصبحت NOMA ضرورية لشبكات الجيل الخامس (5G) وما بعدها. سنتعمق في المبادئ الأساسية لـ NOMA ونقارنها بالتقنيات التقليدية.</p>

                <div class="space-y-8">
                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">1.1 تطور تقنيات الوصول المتعدد ودور NOMA في شبكات 5G/6G</h4>
                        <p class="text-slate-700 mb-4">شهدت تقنيات الوصول المتعدد تطوراً ملحوظاً، بدءاً من FDMA و TDMA و CDMA، وصولاً إلى OFDMA الذي ساد في الأجيال السابقة. تعتمد هذه التقنيات التقليدية (OMA) على تخصيص موارد منفصلة لكل مستخدم، مما يحد من الكفاءة الطيفية. مع متطلبات 5G و 6G غير المسبوقة (الكفاءة الطيفية العالية، الاتصال الهائل، زمن الاستجابة المنخفض، عدالة المستخدم)، برزت NOMA كحل محوري.</p>
                        <p class="text-slate-700">تمثل NOMA تحولاً جذرياً في فلسفة تصميم الاتصالات اللاسلكية؛ فبينما تسعى OMA إلى تجنب التداخل، تتعمد NOMA إدخال تداخل مُدار ومُتحكم فيه بدقة للسماح لعدة مستخدمين بمشاركة نفس الموارد.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">1.2 المبادئ الأساسية والخصائص الرئيسية لتقنية NOMA</h4>
                        <p class="text-slate-700 mb-4">يكمن المفهوم الأساسي لـ NOMA في قدرتها على خدمة أكثر من مستخدم في نفس الكتلة من الموارد (زمن، تردد، كود، فضاء). يتم تحقيق ذلك عن طريق تعدد الإرسال للمستخدمين في نطاق القدرة (Power-Domain NOMA) أو نطاق الكود (Code-Domain NOMA).</p>
                        <p class="text-slate-700 mb-4">تعتمد وظيفة NOMA على عمليتين رئيسيتين:</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>التشفير المتراكب (Superposition Coding - SC):</strong> يتم دمج إشارات المستخدمين المتعددين عند المرسل.</li>
                            <li><strong>إلغاء التداخل المتتالي (Successive Interference Cancellation - SIC):</strong> تُعد هذه التقنية حجر الزاوية عند المستقبل لفك تشفير الإشارات المتراكبة وتخفيف التداخل.</li>
                        </ul>
                        <p class="text-slate-700">تُظهر NOMA قدرتها على التعامل مع عدد أكبر من المستخدمين مقارنةً بفتحات الموارد المتعامدة، وتوفر تخصيصاً للطاقة للعقد ذات ظروف القناة الضعيفة لزيادة الإنتاجية.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">1.3 مزايا NOMA مقارنة بتقنيات الوصول المتعدد المتعامد (OMA)</h4>
                        <p class="text-slate-700 mb-4">تقدم NOMA العديد من المزايا الجوهرية مقارنة بتقنيات OMA. يوضح الجدول التالي أبرز هذه الفروق:</p>
                        <div class="overflow-x-auto bg-slate-50 rounded-lg shadow-inner p-4">
                            <table class="min-w-full divide-y divide-slate-200">
                                <thead class="bg-slate-100">
                                    <tr>
                                        <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-slate-500 uppercase tracking-wider">الميزة</th>
                                        <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-slate-500 uppercase tracking-wider">OMA</th>
                                        <th scope="col" class="px-6 py-3 text-right text-xs font-medium text-slate-500 uppercase tracking-wider">NOMA</th>
                                    </tr>
                                </thead>
                                <tbody class="bg-white divide-y divide-slate-200">
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">تخصيص الموارد</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">موارد منفصلة لكل مستخدم</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">موارد مشتركة بين عدة مستخدمين</td>
                                    </tr>
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">التداخل البيني</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">ضئيل</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">كبير، ولكنه يُدار</td>
                                    </tr>
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">الكفاءة الطيفية</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">محدودة</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">أعلى</td>
                                    </tr>
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">التعقيد</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">أقل</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">أعلى</td>
                                    </tr>
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">الاتصال الهائل</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">لا يدعم بفعالية</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">يدعم عدداً أكبر من المستخدمين</td>
                                    </tr>
                                    <tr>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-slate-900">عدالة المستخدم</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">قد يفضل القنوات الجيدة</td>
                                        <td class="px-6 py-4 whitespace-nowrap text-sm text-slate-600">يوفر عدالة أفضل</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <p class="text-slate-700 mt-4">تُظهر هذه المقارنة أن NOMA تسمح لعدة مستخدمين بالاستفادة من الموارد بشكل متزامن، مما يحسن الكفاءة الطيفية بشكل كبير.</p>
                    </div>
                </div>

                <div class="mt-10 pt-8 border-t border-slate-200">
                    <h4 class="text-2xl font-bold text-teal-700 mb-6">الأنشطة التفاعلية للمحاضرة 1</h4>
                    
                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner mb-6">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 1.1: مناقشة - لماذا NOMA ضرورية لـ 5G/6G؟</h5>
                        <p class="text-slate-700 mb-4">بناءً على ما تعلمته، اكتب فقرة قصيرة تشرح فيها بأفكارك الخاصة لماذا أصبحت تقنية NOMA حاسمة لتلبية متطلبات شبكات الجيل الخامس والسادس.</p>
                        <textarea id="discussion1-1" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب إجابتك هنا..."></textarea>
                        <button class="mt-3 bg-teal-600 text-white py-2 px-4 rounded-md hover:bg-teal-700 transition-colors">إرسال للمراجعة (محاكاة)</button>
                    </div>

                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 1.2: اختبار قصير - أساسيات NOMA</h5>
                        <div class="mb-4">
                            <p class="font-medium text-slate-800 mb-2">السؤال 1: ما هو المبدأ الأساسي الذي يميز NOMA عن OMA؟</p>
                            <div class="space-y-2">
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q1" data-answer="a">أ) تخصيص موارد زمنية منفصلة لكل مستخدم.</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q1" data-answer="b">ب) تخصيص موارد ترددية منفصلة لكل مستخدم.</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q1" data-answer="c">ج) السماح لعدة مستخدمين بمشاركة نفس الموارد مع إدارة التداخل.</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q1" data-answer="d">د) استخدام تشفير الكود الفريد لكل مستخدم.</div>
                            </div>
                            <div id="q1-feedback" class="quiz-feedback text-sm"></div>
                        </div>
                        <div class="mb-4">
                            <p class="font-medium text-slate-800 mb-2">السؤال 2: أي من العمليات التالية تُعد حجر الزاوية في جانب المستقبل لأنظمة NOMA؟</p>
                            <div class="space-y-2">
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q2" data-answer="a">أ) التشفير المتراكب (SC).</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q2" data-answer="b">ب) إلغاء التداخل المتتالي (SIC).</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q2" data-answer="c">ج) تقسيم التردد المتعامد (OFDM).</div>
                                <div class="quiz-option p-3 border border-slate-300 rounded-md" data-question="q2" data-answer="d">د) تشكيل الحزمة.</div>
                            </div>
                            <div id="q2-feedback" class="quiz-feedback text-sm"></div>
                        </div>
                        <button id="submit-quiz1" class="mt-4 bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 transition-colors">تحقق من الإجابات</button>
                    </div>
                </div>
            </section>

            <!-- Lecture 2 Content -->
            <section id="lecture2" class="lecture-content bg-white p-8 rounded-lg shadow-xl mb-10">
                <h3 class="text-3xl font-bold text-slate-900 mb-6">المحاضرة 2: PD-NOMA، التشفير المتراكب، وإلغاء التداخل المتتالي</h3>
                <p class="text-lg text-slate-600 mb-6">في هذه المحاضرة، سنتعمق في تقنية NOMA في نطاق القدرة (PD-NOMA)، ونشرح بالتفصيل عمليتي التشفير المتراكب (SC) وإلغاء التداخل المتتالي (SIC)، وهما جوهر عمل NOMA.</p>

                <div class="space-y-8">
                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">2.1 NOMA في نطاق القدرة (PD-NOMA): التشفير المتراكب وتخصيص الطاقة</h4>
                        <p class="text-slate-700 mb-4">تُعد PD-NOMA المقاربة التي يتم فيها تخصيص مستويات طاقة مختلفة لعدة مستخدمين على نفس الموارد المتعامدة. يتم تراكب إشارات المستخدمين المتعددين في نطاق القدرة.</p>
                        <p class="text-slate-700 mb-4"><strong>التشفير المتراكب (SC):</strong> تتمثل العملية الأساسية في SC عند المحطة الأساسية، حيث يتم دمج إشارات من مستخدمين مختلفين. يتم ذلك عن طريق ضرب إشارة كل مستخدم بمعامل طاقة قبل جمعها.</p>
                        <p class="text-slate-700 mb-4"><strong>مبادئ تخصيص الطاقة:</strong> يتم تخصيص مستويات الطاقة بناءً على جودة القناة الفردية للمستخدمين. القاعدة العامة هي تخصيص طاقة أعلى للمستخدمين ذوي ظروف القناة الأسوأ وطاقة أقل للمستخدمين ذوي ظروف القناة الأفضل. هذا يضمن العدالة ويسمح بفك تشفير المستخدمين الأضعف أولاً.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">2.2 إلغاء التداخل المتتالي (SIC): المبادئ والخطوات</h4>
                        <p class="text-slate-700 mb-4">SIC هي تقنية حاسمة تُستخدم عند المستقبل لفك تشفير المعلومات المتراكبة. الفكرة الأساسية هي فك تشفير إشارات المستخدمين بشكل متتابع. بعد فك تشفير إشارة مستخدم واحد، يتم طرحها من الإشارة المدمجة قبل فك تشفير إشارة المستخدم التالي. يتم ترتيب المستخدمين وفقاً لقوة إشاراتهم؛ حيث يتم فك تشفير الإشارة الأقوى أولاً.</p>
                        <p class="text-slate-700 mb-4"><strong>خطوات SIC في نظام NOMA لمستخدمين اثنين:</strong></p>
                        <div id="sic-process-lecture2" class="border p-4 rounded-lg space-y-3 bg-slate-50 shadow-inner">
                            <div id="sic-step-0-lecture2" class="sic-step-lecture2 p-2 rounded bg-gray-100 transition-all"><strong>الإشارة المستلمة:</strong> <span class="font-mono text-sm">[إشارة 1 (طاقة عالية) + إشارة 2 (طاقة منخفضة) + ضوضاء]</span></div>
                            <div id="sic-step-1-lecture2" class="sic-step-lecture2 p-2 rounded bg-gray-100 opacity-40 transition-all"><strong>خطوة 1:</strong> فك تشفير المستخدم 1 (الأقوى) مباشرة، معتبراً إشارة 2 كضوضاء.</div>
                            <div id="sic-step-2-lecture2" class="sic-step-lecture2 p-2 rounded bg-gray-100 opacity-40 transition-all"><strong>خطوة 2:</strong> إعادة توليد إشارة 1 وطرحها من الإشارة الأصلية.</div>
                            <div id="sic-step-3-lecture2" class="sic-step-lecture2 p-2 rounded bg-gray-100 opacity-40 transition-all"><strong>خطوة 3:</strong> فك تشفير إشارة 2 من الإشارة المتبقية الخالية من التداخل.</div>
                        </div>
                        <button id="sic-next-step-lecture2" class="mt-4 bg-indigo-600 text-white font-bold py-2 px-4 rounded hover:bg-indigo-700 transition-colors">الخطوة التالية في SIC</button>
                    </div>
                </div>

                <div class="mt-10 pt-8 border-t border-slate-200">
                    <h4 class="text-2xl font-bold text-teal-700 mb-6">الأنشطة التفاعلية للمحاضرة 2</h4>
                    
                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner mb-6">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 2.1: محاكاة مفاهيمية - تخصيص الطاقة و SIC</h5>
                        <p class="text-slate-700 mb-4">تخيل أن لديك مستخدمين، المستخدم أ (قناة قوية) والمستخدم ب (قناة ضعيفة). إذا كانت القدرة الكلية المتاحة هي 100 وحدة. كيف ستقوم بتخصيص الطاقة بينهما في نظام PD-NOMA لضمان عدالة المستخدم؟ اشرح سبب اختيارك.</p>
                        <textarea id="simulation2-1" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب إجابتك هنا..."></textarea>
                        <button class="mt-3 bg-teal-600 text-white py-2 px-4 rounded-md hover:bg-teal-700 transition-colors">إرسال للمراجعة (محاكاة)</button>
                    </div>

                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 2.2: مهمة - تحليل أثر انتشار الخطأ</h5>
                        <p class="text-slate-700 mb-4">في نظام SIC ثنائي المستخدمين، ماذا يحدث إذا حدث خطأ في فك تشفير إشارة المستخدم ذي الطاقة الأعلى (المستخدم الأقوى)؟ اشرح كيف يؤثر هذا على أداء المستخدم الآخر.</p>
                        <textarea id="task2-2" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب إجابتك هنا..."></textarea>
                        <button class="mt-3 bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 transition-colors">إرسال المهمة</button>
                    </div>
                </div>
            </section>

            <!-- Lecture 3 Content -->
            <section id="lecture3" class="lecture-content bg-white p-8 rounded-lg shadow-xl mb-10">
                <h3 class="text-3xl font-bold text-slate-900 mb-6">المحاضرة 3: CD-NOMA، مقاييس الأداء، وتحديات النظام</h3>
                <p class="text-lg text-slate-600 mb-6">في هذه المحاضرة، سنتعرف على NOMA في نطاق الكود (CD-NOMA)، ونقارنها بـ PD-NOMA. كما سنتناول مقاييس الأداء الرئيسية لتقييم أنظمة NOMA، ونستعرض التحديات والقيود التي تواجه تنفيذها العملي.</p>

                <div class="space-y-8">
                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">3.1 CD-NOMA: رموز الانتشار وكشف المستخدمين المتعددين</h4>
                        <p class="text-slate-700 mb-4">تُعد CD-NOMA مقاربة مختلفة عن PD-NOMA، حيث تستخدم رموز انتشار مختلفة للتمييز بين المستخدمين الذين يشاركون نفس موارد الوقت والتردد. تلعب رموز الانتشار الفريدة الخاصة بالمستخدمين دورًا رئيسيًا في هذه التقنية.</p>
                        <p class="text-slate-700 mb-4">يتم استخدام خوارزميات كشف المستخدمين المتعددين (Multi-User Detection - MUD) عند المستقبل لفصل المستخدمين بناءً على رموزهم، على غرار أنظمة CDMA. من الأمثلة البارزة على CD-NOMA تقنية CDMA بالانتشار منخفض الكثافة (LDS-CDMA) والوصول المتعدد بالرمز المتفرق (SCMA).</p>
                        <p class="text-slate-700">تتمتع CD-NOMA بميزة مرونة التشكيل على حساب عرض نطاق الإشارة مقارنة بـ PD-NOMA البسيطة.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">3.2 تحليل مقارن ومفاضلات بين PD-NOMA و CD-NOMA</h4>
                        <p class="text-slate-700 mb-4">كلا المقاربتين تقدمان حلولاً مبتكرة، ولكنهما تختلفان في آليات العمل والمفاضلات:</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>نطاق تعدد الإرسال:</strong> PD-NOMA في نطاق القدرة، CD-NOMA في نطاق الكود.</li>
                            <li><strong>إدارة التداخل:</strong> PD-NOMA عبر SIC، CD-NOMA عبر رموز الانتشار و MUD.</li>
                            <li><strong>التعقيد:</strong> PD-NOMA تعقيد SIC، CD-NOMA تعقيد MUD وتصميم الرموز.</li>
                            <li><strong>الاعتماد على حالة القناة:</strong> PD-NOMA تعتمد على فروق كسب القناة، CD-NOMA تستخدم الرموز.</li>
                        </ul>
                        <p class="text-slate-700">لا توجد مقاربة متفوقة عالمياً؛ يعتمد الاختيار على متطلبات النظام المحددة وظروف القناة.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">3.3 مقاييس الأداء الرئيسية</h4>
                        <p class="text-slate-700 mb-4">تُقيّم أنظمة NOMA بناءً على مقاييس أداء رئيسية تعكس قدرتها على تلبية متطلبات الشبكات الحديثة:</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>الكفاءة الطيفية (SE):</strong> تزيد بشكل كبير من SE.</li>
                            <li><strong>الإنتاجية (Throughput):</strong> تهدف إلى تحقيق إنتاجية إجمالية أعلى.</li>
                            <li><strong>زمن الاستجابة (Latency):</strong> يمكن لـ NOMA تقليل زمن الاستجابة.</li>
                            <li><strong>عدالة المستخدم (User Fairness):</strong> تُحسن عدالة المستخدم.</li>
                            <li><strong>مقاييس أخرى:</strong> أداء الانقطاع، معدل الخطأ في البت (BER)، كفاءة الطاقة (EE).</li>
                        </ul>
                        <p class="text-slate-700 text-center mt-6">مقارنة تحسين الكفاءة الطيفية عند دمج NOMA مع تقنيات أخرى:</p>
                        <div class="chart-container h-80">
                            <canvas id="performanceMetricsChartLecture3"></canvas>
                        </div>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">3.4 التحديات والقيود الرئيسية</h4>
                        <p class="text-slate-700 mb-4">على الرغم من المزايا، يواجه تنفيذ NOMA العديد من التحديات:</p>
                        <div id="challenges-accordion-lecture3" class="space-y-3">
                            <div class="accordion-item bg-white rounded-lg shadow">
                                <button class="accordion-header w-full text-right p-4 font-semibold text-slate-800 flex justify-between items-center" aria-expanded="false">
                                    <span>التعقيد الحسابي</span>
                                    <span class="accordion-icon transition-transform transform">+</span>
                                </button>
                                <div class="accordion-content hidden p-4 border-t border-slate-200">
                                    <p class="text-slate-600"><strong>التحدي:</strong> تتطلب عملية SIC فك تشفير متعدد، مما يزيد من عبء المعالجة والتأخير في أجهزة الاستقبال.<br><strong>التخفيف:</strong> تطوير خوارزميات SIC منخفضة التعقيد، استخدام تجميع المستخدمين، والاستفادة من تسريع الأجهزة.</p>
                                </div>
                            </div>
                            <div class="accordion-item bg-white rounded-lg shadow">
                                <button class="accordion-header w-full text-right p-4 font-semibold text-slate-800 flex justify-between items-center" aria-expanded="false">
                                    <span>انتشار الخطأ في SIC</span>
                                    <span class="accordion-icon transition-transform transform">+</span>
                                </button>
                                <div class="accordion-content hidden p-4 border-t border-slate-200">
                                    <p class="text-slate-600"><strong>التحدي:</strong> أي خطأ في فك تشفير إشارة مستخدم قوي ينتشر ويؤثر على فك تشفير الإشارات اللاحقة.<br><strong>التخفيف:</strong> استخدام رموز قوية لتصحيح الأخطاء (FEC)، تقنيات كشف غير خطية، وتقدير دقيق للقناة.</p>
                                </div>
                            </div>
                            <div class="accordion-item bg-white rounded-lg shadow">
                                <button class="accordion-header w-full text-right p-4 font-semibold text-slate-800 flex justify-between items-center" aria-expanded="false">
                                    <span>تقدير القناة الدقيق</span>
                                    <span class="accordion-icon transition-transform transform">+</span>
                                </button>
                                <div class="accordion-content hidden p-4 border-t border-slate-200">
                                    <p class="text-slate-600"><strong>التحدي:</strong> تعتمد NOMA بشكل كبير على معلومات حالة القناة (CSI) الدقيقة لتخصيص الطاقة.<br><strong>التخفيف:</strong> تصميم إشارات مرجعية فعالة، آليات تغذية راجعة قوية، واستخدام تقنيات الذكاء الاصطناعي.</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="mt-10 pt-8 border-t border-slate-200">
                    <h4 class="text-2xl font-bold text-teal-700 mb-6">الأنشطة التفاعلية للمحاضرة 3</h4>
                    
                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner mb-6">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 3.1: مناظرة - PD-NOMA مقابل CD-NOMA</h5>
                        <p class="text-slate-700 mb-4">تخيل أنك مهندس شبكات. في أي سيناريو (على سبيل المثال، عدد كبير من أجهزة إنترنت الأشياء، أو اتصالات النطاق العريض عالية السرعة) قد تفضل CD-NOMA على PD-NOMA، أو العكس؟ اشرح أسبابك.</p>
                        <textarea id="debate3-1" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب إجابتك هنا..."></textarea>
                        <button class="mt-3 bg-teal-600 text-white py-2 px-4 rounded-md hover:bg-teal-700 transition-colors">إرسال للمراجعة (محاكاة)</button>
                    </div>

                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 3.2: مهمة - تحليل أداء الرسم البياني</h5>
                        <p class="text-slate-700 mb-4">بالنظر إلى الرسم البياني "مقاييس الأداء الرئيسية" أعلاه، ما الذي تستنتجه حول تأثير دمج MIMO و RIS مع NOMA على الكفاءة الطيفية؟ وما هي الآثار المترتبة على هذا التحسين؟</p>
                        <textarea id="task3-2" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب إجابتك هنا..."></textarea>
                        <button class="mt-3 bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 transition-colors">إرسال المهمة</button>
                    </div>
                </div>
            </section>

            <!-- Lecture 4 Content -->
            <section id="lecture4" class="lecture-content bg-white p-8 rounded-lg shadow-xl mb-10">
                <h3 class="text-3xl font-bold text-slate-900 mb-6">المحاضرة 4: تكاملات NOMA المتقدمة (MIMO, RIS, AI) والمخططات التعاونية</h3>
                <p class="text-lg text-slate-600 mb-6">تتعمق هذه المحاضرة في كيفية دمج NOMA مع التقنيات اللاسلكية المتقدمة مثل MIMO، والأسطح الذكية القابلة لإعادة التشكيل (RIS)، والذكاء الاصطناعي (AI)، بالإضافة إلى مفاهيم الاتصالات التعاونية والمعرفية.</p>

                <div class="space-y-8">
                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">4.1 MIMO-NOMA: تعزيز السعة والتغطية</h4>
                        <p class="text-slate-700 mb-4">يُعد دمج أنظمة الإدخال المتعدد والإخراج المتعدد (MIMO) مع NOMA استراتيجية قوية لتعزيز أداء الشبكات اللاسلكية. تستفيد هذه التركيبة من تعدد الإرسال المكاني ومكاسب التنوع التي توفرها MIMO.</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>الفوائد:</strong> زيادة السعة والكفاءة الطيفية، تحسين التغطية، تقليل التداخل، تحسين BER وأداء الانقطاع.</li>
                            <li><strong>التحديات:</strong> التعقيد الحسابي العالي، الحاجة للبحث في مصفوفات القنوات ذات الرتبة المنخفضة والتحسين المشترك.</li>
                        </ul>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">4.2 RIS-NOMA: الاستفادة من الأسطح الذكية القابلة لإعادة التشكيل</h4>
                        <p class="text-slate-700 mb-4">تُعد الأسطح الذكية القابلة لإعادة التشكيل (RIS) تقنية لاسلكية ناشئة لديها القدرة على إعادة تشكيل بيئة الانتشار اللاسلكي بشكل سلبي. يمكن دمج RIS مع أنظمة NOMA لتعزيز أدائها.</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>الفوائد:</strong> قمع التداخل، تمديد التغطية، تعديل ترتيب فك التشفير، تحسين الكفاءة الطيفية وكفاءة الطاقة، فعالية التكلفة، أمن الطبقة المادية.</li>
                        </ul>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">4.3 NOMA في الاتصالات التعاونية والمعرفية</h4>
                        <p class="text-slate-700 mb-4">يمكن تعزيز أداء NOMA بشكل أكبر من خلال دمجها مع مفاهيم الاتصالات التعاونية والمعرفية.</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>NOMA التعاونية:</strong> تحسين أداء المستخدمين (خاصة حافة الخلية) من خلال المرحلات التعاونية.</li>
                            <li><strong>NOMA المعرفية:</strong> السماح بالوصول الديناميكي إلى الطيف وتحسين استخدام الموارد (مثلاً في V2X).</li>
                        </ul>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">4.4 دور الذكاء الاصطناعي (AI) والتعلم الآلي في تحسين NOMA</h4>
                        <p class="text-slate-700 mb-4">يمكن تطبيق تقنيات الذكاء الاصطناعي (AI) والتعلم الآلي (ML) لتحسين جوانب مختلفة من أنظمة NOMA، مما يعالج تعقيداتها المتأصلة ويُعزز أدائها.</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>التطبيقات:</strong> تحسين تخصيص الطاقة، مزاوجة/تجميع المستخدمين، تقدير القناة، إدارة التداخل، تخصيص الموارد.</li>
                        </ul>
                    </div>
                </div>

                <div class="mt-10 pt-8 border-t border-slate-200">
                    <h4 class="text-2xl font-bold text-teal-700 mb-6">الأنشطة التفاعلية للمحاضرة 4</h4>
                    
                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner mb-6">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 4.1: عصف ذهني - كيف يمكن للذكاء الاصطناعي حل تحدي مزاوجة المستخدمين الديناميكية في NOMA؟</h5>
                        <p class="text-slate-700 mb-4">صف باختصار كيف يمكن لخوارزميات الذكاء الاصطناعي/التعلم الآلي أن تساهم في تحسين عملية مزاوجة المستخدمين في أنظمة NOMA، مع الأخذ في الاعتبار ظروف القناة المتغيرة وحركة المرور.</p>
                        <textarea id="brainstorm4-1" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب أفكارك هنا..."></textarea>
                        <button class="mt-3 bg-teal-600 text-white py-2 px-4 rounded-md hover:bg-teal-700 transition-colors">إرسال للمراجعة (محاكاة)</button>
                    </div>

                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 4.2: مهمة - تحدي تصميم RIS-NOMA</h5>
                        <p class="text-slate-700 mb-4">تخيل سيناريو حيث يوجد مستخدم على حافة الخلية يعاني من ضعف في الإشارة. كيف يمكن لدمج RIS مع NOMA أن يساعد في تحسين جودة الإشارة لهذا المستخدم؟ ارسم مخططًا بسيطًا (وصفًا نصيًا) يوضح ذلك.</p>
                        <textarea id="task4-2" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب وصفك هنا..."></textarea>
                        <button class="mt-3 bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 transition-colors">إرسال المهمة</button>
                    </div>
                </div>
            </section>

            <!-- Lecture 5 Content -->
            <section id="lecture5" class="lecture-content bg-white p-8 rounded-lg shadow-xl mb-10">
                <h3 class="text-3xl font-bold text-slate-900 mb-6">المحاضرة 5: تطبيقات NOMA، دراسات الحالة، والآفاق المستقبلية</h3>
                <p class="text-lg text-slate-600 mb-6">تستعرض هذه المحاضرة التطبيقات العملية لـ NOMA في شبكات 5G وما بعدها، مع التركيز على دراسات حالة محددة. كما سنناقش مشاكل البحث المفتوحة والاتجاهات الناشئة التي تشكل مستقبل NOMA.</p>

                <div class="space-y-8">
                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">5.1 NOMA في شبكات 5G وما بعدها (6G)</h4>
                        <p class="text-slate-700 mb-4">تُعد NOMA تقنية أساسية لتمكين شبكات 5G، حيث تدعم الاتصال الهائل، وزيادة السعة، وتقليل زمن الاستجابة، وتحسين تجربة المستخدم. كما تُعد مرشحاً قوياً لتلبية العديد من متطلبات 5G وما بعدها.</p>
                        <p class="text-slate-700">يستكشف الباحثون إمكانات NOMA لشبكات 6G، والتي من المتوقع أن تتطلب سعة أعلى وزمن استجابة أقل، وتدمج الاتصالات الأرضية والجوية والبحرية والفضائية في شبكة عالمية.</p>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">5.2 تطبيقات محددة: V2X، الاتصالات عبر الأقمار الصناعية، وإنترنت الأشياء (IoT)</h4>
                        <p class="text-slate-700 mb-4">تُظهر NOMA إمكانات كبيرة في مجموعة واسعة من التطبيقات المتطورة:</p>
                        <div class="grid sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mt-4">
                            <div class="integration-card text-center p-6 bg-slate-100 rounded-lg hover:shadow-xl transition-all">
                                <div class="text-4xl mb-3 text-teal-600">🚗</div>
                                <h5 class="font-bold text-lg text-slate-800">الاتصال من مركبة إلى كل شيء (V2X)</h5>
                                <p class="text-sm text-slate-600 mt-2">تُمكّن الاتصالات ذات زمن الاستجابة المنخفض والموثوقية العالية في البيئات المرورية الكثيفة.</p>
                            </div>
                            <div class="integration-card text-center p-6 bg-slate-100 rounded-lg hover:shadow-xl transition-all">
                                <div class="text-4xl mb-3 text-teal-600">🛰️</div>
                                <h5 class="font-bold text-lg text-slate-800">الاتصالات عبر الأقمار الصناعية</h5>
                                <p class="text-sm text-slate-600 mt-2">تحسين الكفاءة الطيفية ودعم العديد من المستخدمين في الشبكات المتكاملة الفضائية-الأرضية.</p>
                            </div>
                            <div class="integration-card text-center p-6 bg-slate-100 rounded-lg hover:shadow-xl transition-all">
                                <div class="text-4xl mb-3 text-teal-600">💡</div>
                                <h5 class="font-bold text-lg text-slate-800">إنترنت الأشياء (IoT)</h5>
                                <p class="text-sm text-slate-600 mt-2">دعم عدد كبير من أجهزة إنترنت الأشياء، وهو أمر بالغ الأهمية لاتصالات الآلة الضخمة.</p>
                            </div>
                        </div>
                    </div>

                    <div>
                        <h4 class="text-2xl font-semibold text-indigo-700 mb-4">5.3 مشاكل البحث المفتوحة والاتجاهات الناشئة</h4>
                        <p class="text-slate-700 mb-4">لا تزال NOMA مجالاً نشطاً للبحث والتطوير، مع العديد من المشاكل المفتوحة والاتجاهات الناشئة:</p>
                        <ul class="list-disc list-inside text-slate-700 ml-4 mb-4">
                            <li><strong>القضايا الأمنية:</strong> تُعد أنظمة NOMA عرضة للقضايا الأمنية، مما يتطلب المزيد من التحقيق.</li>
                            <li><strong>المزاوجة/التجميع الديناميكي للمستخدمين:</strong> مهمة معقدة لتحقيق أقصى قدر من الفوائد.</li>
                            <li><strong>نماذج القنوات العملية:</strong> الانتقال إلى نماذج أكثر واقعية تتجاوز تلاشي رايلي.</li>
                            <li><strong>قابلية توسع النظام:</strong> المتانة لدعم حركة المرور غير المتجانسة وبيئات الراديو المتنوعة.</li>
                            <li><strong>العروض العملية:</strong> الحاجة إلى المزيد من التجارب عبر الهواء.</li>
                            <li><strong>الاتجاهات الناشئة:</strong> التكامل مع الذكاء الاصطناعي، متغيرات NOMA الجديدة، NOMA لشبكات 6G.</li>
                        </ul>
                    </div>
                </div>

                <div class="mt-10 pt-8 border-t border-slate-200">
                    <h4 class="text-2xl font-bold text-teal-700 mb-6">الأنشطة التفاعلية للمحاضرة 5</h4>
                    
                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner mb-6">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 5.1: مشروع جماعي (محاكاة) - اقتراح تطبيق NOMA جديد</h5>
                        <p class="text-slate-700 mb-4">تخيل أنك جزء من فريق تطوير. اقترح تطبيقًا جديدًا لتقنية NOMA لم يتم ذكره في المحاضرة. صف هذا التطبيق، واشرح كيف يمكن لـ NOMA أن تحسن من أدائه، وما هي التحديات التي قد تواجهونها في تنفيذه.</p>
                        <textarea id="project5-1" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="8" placeholder="اكتب اقتراحك هنا..."></textarea>
                        <button class="mt-3 bg-teal-600 text-white py-2 px-4 rounded-md hover:bg-teal-700 transition-colors">إرسال المشروع (محاكاة)</button>
                    </div>

                    <div class="bg-slate-50 p-6 rounded-lg shadow-inner">
                        <h5 class="text-xl font-semibold text-indigo-800 mb-4">نشاط 5.2: حلقة نقاش (محاكاة) - أكبر العقبات أمام انتشار NOMA</h5>
                        <p class="text-slate-700 mb-4">بالنظر إلى كل ما تعلمته، ما هي في رأيك أكبر عقبة (أو عقبتين) أمام انتشار تقنية NOMA على نطاق واسع في الشبكات اللاسلكية التجارية؟ وما هي الخطوات التي يمكن اتخاذها للتغلب على هذه العقبات؟</p>
                        <textarea id="discussion5-2" class="w-full p-3 border border-slate-300 rounded-md focus:ring-teal-500 focus:border-teal-500" rows="5" placeholder="اكتب وجهة نظرك هنا..."></textarea>
                        <button class="mt-3 bg-indigo-600 text-white py-2 px-4 rounded-md hover:bg-indigo-700 transition-colors">إرسال للمراجعة (محاكاة)</button>
                    </div>
                </div>
            </section>

        </main>
    </div>
    
    <footer class="bg-slate-800 text-white mt-16">
        <div class="container mx-auto py-6 px-4 sm:px-6 lg:px-8 text-center text-sm">
            <p>&copy; 2025 حقيبة تدريب NOMA التفاعلية. تم إنشاؤها لتبسيط المفاهيم المعقدة في الاتصالات.</p>
            <p>تم استخلاص المحتوى من تقرير "تصميم حقيبة تدريبية علمية تفاعلية شاملة عن تقنية الوصول المتعدد غير المتعامد (NOMA)".</p>
        </div>
    </footer>

<script>
document.addEventListener('DOMContentLoaded', function() {

    const lectureLinks = document.querySelectorAll('.lecture-link');
    const lectureContents = document.querySelectorAll('.lecture-content');

    function showLecture(lectureId) {
        lectureContents.forEach(content => {
            content.classList.remove('active');
        });
        document.getElementById(lectureId).classList.add('active');

        lectureLinks.forEach(link => {
            link.classList.remove('active-lecture-link');
        });
        document.querySelector(`[data-lecture="${lectureId}"]`).classList.add('active-lecture-link');
    }

    lectureLinks.forEach(link => {
        link.addEventListener('click', function(e) {
            e.preventDefault();
            const lectureId = this.dataset.lecture;
            showLecture(lectureId);
        });
    });

    // Initialize the first lecture as active
    showLecture('lecture1');

    // Quiz Logic (Lecture 1)
    const quizOptions = document.querySelectorAll('.quiz-option');
    const submitQuiz1Button = document.getElementById('submit-quiz1');
    const correctAnswers = {
        q1: 'c',
        q2: 'b'
    };
    let selectedAnswers = {};

    quizOptions.forEach(option => {
        option.addEventListener('click', function() {
            const questionId = this.dataset.question;
            const answerId = this.dataset.answer;

            // Remove selected class from other options for the same question
            document.querySelectorAll(`.quiz-option[data-question="${questionId}"]`).forEach(opt => {
                opt.classList.remove('selected', 'correct', 'incorrect');
            });

            // Add selected class to the clicked option
            this.classList.add('selected');
            selectedAnswers[questionId] = answerId;

            // Clear previous feedback
            document.getElementById(`${questionId}-feedback`).textContent = '';
        });
    });

    submitQuiz1Button.addEventListener('click', function() {
        for (const questionId in correctAnswers) {
            const feedbackElement = document.getElementById(`${questionId}-feedback`);
            const optionsForQuestion = document.querySelectorAll(`.quiz-option[data-question="${questionId}"]`);
            
            if (selectedAnswers[questionId] === correctAnswers[questionId]) {
                feedbackElement.textContent = 'إجابة صحيحة!';
                feedbackElement.style.color = '#059669'; // Emerald-600
                optionsForQuestion.forEach(opt => {
                    if (opt.dataset.answer === selectedAnswers[questionId]) {
                        opt.classList.add('correct');
                    }
                });
            } else {
                feedbackElement.textContent = 'إجابة خاطئة. الإجابة الصحيحة هي: ' + correctAnswers[questionId].toUpperCase();
                feedbackElement.style.color = '#ef4444'; // Red-500
                optionsForQuestion.forEach(opt => {
                    if (opt.dataset.answer === selectedAnswers[questionId]) {
                        opt.classList.add('incorrect');
                    }
                    if (opt.dataset.answer === correctAnswers[questionId]) {
                        opt.classList.add('correct'); // Highlight correct answer
                    }
                });
            }
        }
    });

    // SIC Process Logic (Lecture 2)
    let sicCurrentStepLecture2 = 0;
    const sicStepsLecture2 = document.querySelectorAll('#sic-process-lecture2 .sic-step-lecture2');
    const sicNextButtonLecture2 = document.getElementById('sic-next-step-lecture2');

    function updateSicStepLecture2() {
        sicStepsLecture2.forEach((step, index) => {
            if (index <= sicCurrentStepLecture2) {
                step.classList.remove('opacity-40');
                step.classList.add('bg-teal-100', 'text-teal-800');
            } else {
                step.classList.add('opacity-40');
                step.classList.remove('bg-teal-100', 'text-teal-800');
            }
        });
        if (sicCurrentStepLecture2 >= sicStepsLecture2.length - 1) {
            sicNextButtonLecture2.textContent = 'إعادة العملية';
        } else {
            sicNextButtonLecture2.textContent = 'الخطوة التالية في SIC';
        }
    }

    sicNextButtonLecture2.addEventListener('click', () => {
        sicCurrentStepLecture2++;
        if (sicCurrentStepLecture2 >= sicStepsLecture2.length) {
            sicCurrentStepLecture2 = 0;
        }
        updateSicStepLecture2();
    });
    updateSicStepLecture2(); // Initial state


    // Performance Metrics Chart (Lecture 3)
    const performanceMetricsCtxLecture3 = document.getElementById('performanceMetricsChartLecture3').getContext('2d');
    new Chart(performanceMetricsCtxLecture3, {
        type: 'bar',
        data: {
            labels: ['NOMA الأساسية', 'MIMO-NOMA', 'RIS-NOMA'],
            datasets: [{
                label: 'تحسين الكفاءة الطيفية (مقارنة بـ OMA)',
                data: [30, 81, 120],
                backgroundColor: [
                    'rgba(13, 148, 136, 0.6)',
                    'rgba(79, 70, 229, 0.6)',
                    'rgba(217, 70, 239, 0.6)',
                ],
                borderColor: [
                    'rgb(13, 148, 136)',
                    'rgb(79, 70, 229)',
                    'rgb(217, 70, 239)',
                ],
                borderWidth: 1
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                y: {
                    beginAtZero: true,
                    title: {
                        display: true,
                        text: 'التحسين (%)',
                        font: { family: "'Cairo', sans-serif" }
                    }
                },
                x: {
                   ticks: { font: { family: "'Cairo', sans-serif" } }
                }
            },
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return 'تحسين بنسبة ' + context.raw + '%';
                        },
                        bodyFont: { family: "'Cairo', sans-serif" },
                        titleFont: { family: "'Cairo', sans-serif" }
                    }
                }
            }
        }
    });

    // Accordion Logic (Lecture 3)
    const accordionItemsLecture3 = document.querySelectorAll('#challenges-accordion-lecture3 .accordion-item');
    accordionItemsLecture3.forEach(item => {
        const header = item.querySelector('.accordion-header');
        const content = item.querySelector('.accordion-content');
        const icon = item.querySelector('.accordion-icon');

        header.addEventListener('click', () => {
            const isVisible = !content.classList.contains('hidden');
            content.classList.toggle('hidden');
            header.setAttribute('aria-expanded', !isVisible);
            icon.classList.toggle('rotate-45', !isVisible);
        });
    });

    // Generic "Submit for Review" buttons (for tasks/discussions)
    document.querySelectorAll('button.bg-teal-600, button.bg-indigo-600').forEach(button => {
        if (button.id !== 'submit-quiz1' && button.id !== 'sic-next-step-lecture2') {
            button.addEventListener('click', function() {
                alert('تم إرسال إجابتك للمراجعة. في نظام تدريب حقيقي، سيتم حفظ إجابتك هنا وتقديم ملاحظات.');
            });
        }
    });
});
</script>

</body>
</html>
# noma
