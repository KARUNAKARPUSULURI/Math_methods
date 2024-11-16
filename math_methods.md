```javascript
// Continuing Example 3: Date Range Iterator
function* dateRangeGenerator(startDate, endDate, { skipWeekends = false, skipHolidays = [] } = {}) {
    const current = new Date(startDate);
    const end = new Date(endDate);
    
    while (current <= end) {
        const isWeekend = current.getDay() === 0 || current.getDay() === 6;
        const isHoliday = skipHolidays.some(holiday => 
            holiday.getDate() === current.getDate() &&
            holiday.getMonth() === current.getMonth()
        );
        
        if ((!skipWeekends || !isWeekend) && (!isHoliday)) {
            yield new Date(current);
        }
        
        current.setDate(current.getDate() + 1);
    }
}

// Usage example
const holidays = [
    new Date("2024-12-25"), // Christmas
    new Date("2024-12-31")  // New Year's Eve
];

const dateRange = dateRangeGenerator(
    "2024-12-20",
    "2024-12-31",
    { skipWeekends: true, skipHolidays: holidays }
);

console.log("Working days in range:");
for (const date of dateRange) {
    console.log(date.toDateString());
}

// Example 4: Age Calculator with Extra Details
function calculateDetailedAge(birthDate) {
    const birth = new Date(birthDate);
    const now = new Date();
    
    const yearDiff = now.getFullYear() - birth.getFullYear();
    const monthDiff = now.getMonth() - birth.getMonth();
    const dayDiff = now.getDate() - birth.getDate();
    
    let ageYears = yearDiff;
    let ageMonths = monthDiff;
    let ageDays = dayDiff;
    
    if (dayDiff < 0) {
        ageMonths--;
        const lastMonth = new Date(now.getFullYear(), now.getMonth() - 1, birth.getDate());
        ageDays = Math.floor((now - lastMonth) / (1000 * 60 * 60 * 24));
    }
    
    if (monthDiff < 0 || (monthDiff === 0 && dayDiff < 0)) {
        ageYears--;
        ageMonths += 12;
    }
    
    const nextBirthday = new Date(now.getFullYear(), birth.getMonth(), birth.getDate());
    if (nextBirthday < now) {
        nextBirthday.setFullYear(nextBirthday.getFullYear() + 1);
    }
    
    const daysToNextBirthday = Math.ceil((nextBirthday - now) / (1000 * 60 * 60 * 24));
    
    return {
        years: ageYears,
        months: ageMonths,
        days: ageDays,
        totalMonths: ageYears * 12 + ageMonths,
        totalDays: Math.floor((now - birth) / (1000 * 60 * 60 * 24)),
        nextBirthday: {
            date: nextBirthday,
            daysUntil: daysToNextBirthday,
            dayOfWeek: nextBirthday.toLocaleDateString('en-US', { weekday: 'long' })
        },
        zodiacSign: getZodiacSign(birth),
        isLeapYearBirth: isLeapYear(birth.getFullYear())
    };
}

function getZodiacSign(date) {
    const month = date.getMonth() + 1;
    const day = date.getDate();
    
    if ((month === 3 && day >= 21) || (month === 4 && day <= 19)) return "Aries";
    if ((month === 4 && day >= 20) || (month === 5 && day <= 20)) return "Taurus";
    if ((month === 5 && day >= 21) || (month === 6 && day <= 20)) return "Gemini";
    if ((month === 6 && day >= 21) || (month === 7 && day <= 22)) return "Cancer";
    if ((month === 7 && day >= 23) || (month === 8 && day <= 22)) return "Leo";
    if ((month === 8 && day >= 23) || (month === 9 && day <= 22)) return "Virgo";
    if ((month === 9 && day >= 23) || (month === 10 && day <= 22)) return "Libra";
    if ((month === 10 && day >= 23) || (month === 11 && day <= 21)) return "Scorpio";
    if ((month === 11 && day >= 22) || (month === 12 && day <= 21)) return "Sagittarius";
    if ((month === 12 && day >= 22) || (month === 1 && day <= 19)) return "Capricorn";
    if ((month === 1 && day >= 20) || (month === 2 && day <= 18)) return "Aquarius";
    return "Pisces";
}

function isLeapYear(year) {
    return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

console.log("Detailed Age:", calculateDetailedAge("1990-05-15"));

// Example 5: Smart Event Scheduler
function createEventScheduler(eventConfig) {
    return {
        generateSchedule(startDate, occurrences) {
            const schedule = [];
            let currentDate = new Date(startDate);
            
            while (schedule.length < occurrences) {
                if (this.isValidEventDay(currentDate)) {
                    schedule.push({
                        date: new Date(currentDate),
                        formattedDate: currentDate.toLocaleDateString(),
                        dayOfWeek: currentDate.toLocaleDateString('en-US', { weekday: 'long' }),
                        time: currentDate.toLocaleTimeString(),
                        type: this.getEventType(currentDate)
                    });
                }
                currentDate.setDate(currentDate.getDate() + 1);
            }
            
            return schedule;
        },
        
        isValidEventDay(date) {
            const dayOfWeek = date.getDay();
            const isWeekend = dayOfWeek === 0 || dayOfWeek === 6;
            
            if (eventConfig.weekdaysOnly && isWeekend) return false;
            if (eventConfig.excludeDates && this.isExcludedDate(date)) return false;
            if (eventConfig.timeRange && !this.isWithinTimeRange(date)) return false;
            
            return true;
        },
        
        isExcludedDate(date) {
            return eventConfig.excludeDates.some(excludedDate => 
                excludedDate.getDate() === date.getDate() &&
                excludedDate.getMonth() === date.getMonth() &&
                excludedDate.getFullYear() === date.getFullYear()
            );
        },
        
        isWithinTimeRange(date) {
            const hours = date.getHours();
            return hours >= eventConfig.timeRange.start && 
                   hours <= eventConfig.timeRange.end;
        },
        
        getEventType(date) {
            const hours = date.getHours();
            if (hours < 12) return 'Morning Session';
            if (hours < 17) return 'Afternoon Session';
            return 'Evening Session';
        }
    };
}

// Usage example
const eventConfig = {
    weekdaysOnly: true,
    timeRange: { start: 9, end: 17 },
    excludeDates: [
        new Date("2024-12-25"),
        new Date("2024-12-31")
    ]
};

const scheduler = createEventScheduler(eventConfig);
const eventSchedule = scheduler.generateSchedule(new Date(), 5);
console.log("Event Schedule:", eventSchedule);

// Example 6: Date Formatter with Templates
function createDateFormatter(templates = {}) {
    const defaultTemplates = {
        short: 'MM/DD/YYYY',
        medium: 'MMM DD, YYYY',
        long: 'MMMM DD, YYYY',
        full: 'dddd, MMMM DD, YYYY'
    };

    const formatTokens = {
        YYYY: date => date.getFullYear(),
        MM: date => String(date.getMonth() + 1).padStart(2, '0'),
        DD: date => String(date.getDate()).padStart(2, '0'),
        MMM: date => date.toLocaleString('en', { month: 'short' }),
        MMMM: date => date.toLocaleString('en', { month: 'long' }),
        dd: date => String(date.getDate()),
        dddd: date => date.toLocaleString('en', { weekday: 'long' }),
        HH: date => String(date.getHours()).padStart(2, '0'),
        mm: date => String(date.getMinutes()).padStart(2, '0'),
        ss: date => String(date.getSeconds()).padStart(2, '0')
    };

    return {
        format(date, templateName = 'medium') {
            const template = templates[templateName] || defaultTemplates[templateName] || templateName;
            
            return template.replace(/YYYY|MM|DD|MMM|MMMM|dd|dddd|HH|mm|ss/g, match => 
                formatTokens[match](date)
            );
        },
        
        formatRange(startDate, endDate, templateName = 'medium') {
            return `${this.format(startDate, templateName)} - ${this.format(endDate, templateName)}`;
        }
    };
}

const formatter = createDateFormatter({
    custom: 'dddd, MMM DD, YYYY at HH:mm',
    dateOnly: 'MM/DD/YYYY',
    timeOnly: 'HH:mm:ss'
});

const date = new Date();
console.log("Formatted Dates:");
console.log("Short:", formatter.format(date, 'short'));
console.log("Custom:", formatter.format(date, 'custom'));
console.log("Time Only:", formatter.format(date, 'timeOnly'));
```

These examples demonstrate advanced date manipulation and formatting techniques, including:
1. Date range generation with weekend and holiday exclusions
2. Detailed age calculation with zodiac sign and leap year information
3. Smart event scheduling system with configuration options
4. Custom date formatting system with templates

Each example includes practical use cases and can be extended or modified based on specific needs. Would you like me to explain any particular example in more detail or add more specific use cases?