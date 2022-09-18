package com.bridgelabz;

import java.time.DayOfWeek;
import java.time.LocalDate;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class HotelReservationService {

    List<Hotel> hotelList = new ArrayList<>();

    boolean addHotel(Hotel hotel) {
        hotelList.add(hotel);
        return true;
    }

    Hotel getCheapestBestRatedHotel(String checkInDate, String checkOutDate,boolean isRewardedCustomer) {
        if (isRewardedCustomer) {
            hotelList.forEach(hotel ->{
                hotel.setWeekdayRate(hotel.getWeekdayRewardedRate());
                hotel.setWeekendRate(hotel.getWeekendRewardedRate());
            } );
        }
        LocalDate inDate = LocalDate.of(Integer.valueOf(checkInDate.substring(6, 10)), Integer.valueOf(checkInDate.substring(3, 5)), Integer.valueOf(checkInDate.substring(0, 2)));
        LocalDate outDate = LocalDate.of(Integer.valueOf(checkOutDate.substring(6, 10)), Integer.valueOf(checkOutDate.substring(3, 5)), Integer.valueOf(checkOutDate.substring(0, 2)));

        long totalNumberOfDays = getNumberOfDays(inDate, outDate);
        long weekendDays = getNumberOfWeekendDays(inDate, outDate);
        long weekDays = totalNumberOfDays - weekendDays;

        calculateTotalCost(weekDays, weekendDays);
        List<Hotel> hotelList1 = hotelList.stream().sorted((x, y) -> Long.compare(x.getTotalCost(), y.getTotalCost())).collect(Collectors.toList());
        List<Hotel> hotelList2 = hotelList1.stream().filter(x -> x.getTotalCost() == hotelList1.get(0).getTotalCost()).sorted((x, y) -> Integer.compare(y.getRatings(), x.getRatings())).collect(Collectors.toList());

        return hotelList2.get(0);
    }

    Hotel getBestRatedHotel() {
        return hotelList.stream().sorted((x, y) -> Integer.compare(y.getRatings(), x.getRatings())).collect(Collectors.toList()).get(0);
    }

    long getNumberOfDays(LocalDate checkInDate, LocalDate checkOutDate) {
        return (int) checkInDate.datesUntil(checkOutDate).count();
    }

    long getNumberOfWeekendDays(LocalDate checkInDate, LocalDate checkOutDate) {
        Stream<LocalDate> weekendDays = checkInDate.datesUntil(checkOutDate).filter(date -> {
            DayOfWeek day = date.getDayOfWeek();
            boolean isWeekendDay = (day == DayOfWeek.SATURDAY || day == DayOfWeek.SUNDAY);
            return isWeekendDay;
        });
        return weekendDays.collect(Collectors.toList()).size();
    }

    void calculateTotalCost(long weekDays, long weekendDays) {
        hotelList.stream().forEach(x -> {
            x.setTotalCost(weekDays * x.getWeekdayRate() + weekendDays * x.getWeekendRate());
        });
    }
}
