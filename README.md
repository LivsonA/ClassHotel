# ClassHotel
Система классов для управления бронированием номеров в отеле.


Класс Room (Номер)

Атрибуты:
number: int — номер комнаты;
type: str — тип номера, одно из: 'single', 'double', 'suite';
price_per_night: float — цена за ночь;
amenities: list[str] — удобства в номере (по умолчанию пустой список).


Класс Guest (Гость)

Атрибуты:
id: int — идентификатор гостя;
name: str — имя;
email: str — email;
phone: str — телефон.


Класс Booking (Бронирование)

Атрибуты:
id: int — идентификатор бронирования;
guest: Guest — гость;
room: Room — забронированный номер;
check_in: date —- дата заезда;
check_out: date — дата выезда;
status: str — статус бронирования, одно из: 'confirmed', 'checked_in', 'checked_out', 'cancelled' (по умолчанию 'confirmed').

Методы:
duration() -> int — возвращает количество ночей (разница между check_out и check_in);
total_price() -> float — возвращает общую стоимость = price_per_night * duration();
is_active_on(date: date) -> bool — возвращает True, если указанная дата находится между check_in и check_out (включая начало и не включая конец);
can_cancel(reference_date: date) -> bool — возвращает True, если до даты заезда осталось больше 48 часов относительно reference_date;
check_in_guest() -> None — меняет статус на 'checked_in' (только если статус 'confirmed');
check_out_guest() -> None — меняет статус на 'checked_out' (только если статус 'checked_in').


Класс Hotel (Отель)

Атрибуты:
name: str — название отеля;
rooms: list[Room] — список номеров отеля (по умолчанию пустой список);
bookings: list[Booking] — список бронирований (по умолчанию пустой список).

Методы:
add_room(room: Room) -> None — добавляет номер в отель;
book_room(guest: Guest, room_type: str, check_in: date, check_out: date) -> Optional[Booking] — ищет свободный номер указанного типа на заданные даты; если номер найден, создает бронирование и возвращает его; если нет свободных номеров, возвращает None.
available_rooms(date_range: tuple[date, date], room_type: Optional[str] = None) -> list[Room] — возвращает список номеров, свободных в указанный период; если room_type указан, фильтрует по типу.
occupancy_rate(date: date) -> float — возвращает процент занятых номеров на указанную дату; занятыми считаются номера с активным бронированием (статус 'confirmed' или 'checked_in' на эту дату).
