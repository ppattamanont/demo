#!/usr/bin/env python3


from sqlalchemy import create_engine
from sqlalchemy import Table, Column, Integer, String, MetaData, ForeignKey
from sqlalchemy import DateTime, Boolean

# Create a sqlite database 
engine = create_engine('sqlite:///flights_pj2.sqlite')

metadata=MetaData()
Airports = Table ('Airports', metadata,
                Column('Code', String, primary_key=True),
                Column('FL_DATE', DateTime),
                Column('AIRLINE_ID', Integer, autoincrement=True),
                Column('ORIGIN', String),
                Column('ORIGIN_CITY_NAME', String),
                Column('DEST', String),
                Column('DEP_DEL15', Boolean),
               )
Airlines = Table ('Airlines', metadata,
                 Column('AIRLINE_ID', Integer, ForeignKey("Airports.AIRLINE_ID")),
                 Column('DISTANCE', Integer),
                 Column('ORIGIN_CITY_NAME', String),
                 Column('DEST', String),
                 )
metadata.create_all(engine)

# Put data in a database 
import csv
flights=open("/ufrc/zoo6927/share/Class_Files/data/flights.May2017-Apr2018.csv")

reader = csv.DictReader(flights)
for Line in reader:

    ins=Airports.insert().values(FL_DATE=Line['FL_DATE'],AIRLINE_ID=Line['AIRLINE_ID'],
                                ORIGIN=Line['ORIGIN'],ORIGIN_CITY_NAME=Line['ORIGIN_CITY_NAME'],DEST=Line['DEST'],
                                DEP_DEL15 = Line['DEP_DEL15'])

result = conn.execute(ins)
