package com.autoclicker.app

import android.os.Parcel
import android.os.Parcelable

data class Selection(
    val slot: Int,
    var num1: Int,
    var num2: Int,
    var num3: Int,
    var name: String = "Selection $slot"
) : Parcelable {
    val isValid get() = num1 in 1..80 && num2 in 1..80 && num3 in 1..80 && num1 != num2 && num1 != num3 && num2 != num3
    fun toDisplayString() = if (isValid) "[$num1 - $num2 - $num3]" else "[Empty]"
    fun toNumbers() = listOf(num1, num2, num3)
    constructor(parcel: Parcel) : this(parcel.readInt(), parcel.readInt(), parcel.readInt(), parcel.readInt(), parcel.readString() ?: "")
    override fun writeToParcel(parcel: Parcel, flags: Int) { parcel.writeInt(slot); parcel.writeInt(num1); parcel.writeInt(num2); parcel.writeInt(num3); parcel.writeString(name) }
    override fun describeContents() = 0
    companion object CREATOR : Parcelable.Creator<Selection> {
        override fun createFromParcel(parcel: Parcel) = Selection(parcel)
        override fun newArray(size: Int): Array<Selection?> = arrayOfNulls(size)
    }
}

data class GridConfig(
    var startX: Float = 0f, var startY: Float = 0f,
    var cellWidth: Float = 60f, var cellHeight: Float = 60f,
    var cols: Int = 10, var rows: Int = 8
) {
    fun getCoordinates(number: Int): Pair<Float, Float> {
        if (number < 1 || number > 80) return Pair(0f, 0f)
        val index = number - 1
        return Pair(startX + (index % cols) * cellWidth + cellWidth / 2, startY + (index / cols) * cellHeight + cellHeight / 2)
    }
}
